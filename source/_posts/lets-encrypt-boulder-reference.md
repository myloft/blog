---
title: reference 与 let's encrypt 证书吊销
date: 2020-03-17 14:50:51
update: 2020-03-17 14:50:51
tags: golang
---
　　最近 `let's encrypt`，宣布吊销 300 万个证书，因为没被波及所以没收到邮件。今天才看到了这条新闻，根据描述是因为 Boulder 的 bug 导致不能正确的验证 `CAA`。
<!-- more -->
## CAA
　　要搞明白这个 bug 得先了解 `CAA` 是干嘛用的。简单的来说 CA 在签发证书的时候需要检查一下域名的 `CAA` 记录，如果有自己的话就签发，如果没有就拒绝签发。通过设置 `CAA` 就可以防止某些人利用其它 CA 的一些漏洞签发证书。比如这次的 `let's encrypt`，`CAA` 需要设置成 `letsencrypt.org`。如果什么都不设，任何 CA 都可以签发证书。
## 漏洞发现
　　了解了 `CAA` 就可以来看一看 `let's encrypt` 论坛上的这个[求助](https://community.letsencrypt.org/t/rechecking-`CAA`-fails-with-99-identical-subproblems/113517)。正确情况下检测多个域名 `CAA` 如果一个域名检测失败，报一次错；多个域名检测失败，报多个不同的错。但实际上 `admin.mrhs.hwrsd.org` 这个域名报错 99 次。并且 let’s encrypt 也的确重复检查了 99 次。

　　也就是说实际上只检测了 `admin.mrhs.hwrsd.org` 一个域名，其它域名并没有被检测。这样就可以使用一个可以通过检测的域名来让那些那些没有正确设置 `CAA` 的域名逃过检测。
## 漏洞代码
　　那问题出在了哪呢，看到这个直白的 pull request 标题就明白了 [Pass authzModel by value, not reference](https://github.com/letsencrypt/boulder/pull/4690/)。
　　看一下那段处理多个域名的代码。
```go
func authzModelMapToPB(m map[string]authzModel) (*sapb.Authorizations, error) {
	resp := &sapb.Authorizations{}
	for k, v := range m {
		// Make a copy of k because it will be reassigned with each loop.
		kCopy := k
		authzPB, err := modelToAuthzPB(&v)
		if err != nil {
			return nil, err
		}
		resp.Authz = append(resp.Authz, &sapb.Authorizations_MapElement{Domain: &kCopy, Authz: authzPB})
	}
	return resp, nil
}
```
　　乍一看这段代码看起来都没有什么问题。循环调用 `modelToAuthzPB` 方法，传入引用 `v` 获得 `authPB`。为循环中会被重新分配的 `k` 创建拷贝。最终将 `k` 与 `authPB` 返回走。

```go
func modelToAuthzPB(am *authzModel) (*corepb.Authorization, error) {
	expires := am.Expires.UTC().UnixNano()
	id := fmt.Sprintf("%d", am.ID)
	status := uintToStatus[am.Status]
	pb := &corepb.Authorization{
		Id:             &id,
		Status:         &status,
		Identifier:     &am.IdentifierValue,
		RegistrationID: &am.RegistrationID,
		Expires:        &expires,
    }
    ...
    return pb, nil
}
```
　　但问题在于所返回的结构体 `pb` 内部的元素也是引用，比如这个 `&am.IdentifierValue`。每次传入的 `v` 是引用，结构体中的元素也是引用。这就导致 `authPB` 看起来不是个引用，实际上它的内部还是个引用。循环结束后，`authPB` 的值都来自于最后一次的 `v` 的结果。也就是说如果最后一个域名通过了检测，那么之前的域名都会通过检测。
## 解决方案
　　知道了原因，那解决起来就简单了，第一种方法直接将 `modelToAuthzPB` 方法从传引用改成传值。第二种方法像 `k` 一样，为 `v` 也搞一个 `vCopy`。而 `let's encrypt` 选择了第一种方法。 
```go
func authzModelMapToPB(m map[string]authzModel) (*sapb.Authorizations, error) {
	resp := &sapb.Authorizations{}
	for k, v := range m {
		// Make a copy of k because it will be reassigned with each loop.
		kCopy := k
		authzPB, err := modelToAuthzPB(v)
		if err != nil {
			return nil, err
		}
		resp.Authz = append(resp.Authz, &sapb.Authorizations_MapElement{Domain: &kCopy, Authz: authzPB})
	}
	return resp, nil
}
```
　　说实话在我自己的项目里各种 reference 乱飞，多半某天也会像这样吃个亏。