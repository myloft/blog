---
title: golang 实现 token 认证
date: 2020-03-06 18:36:43
updated: 2020-03-06 18:36:43
tags: golang
---
　　之前做项目的时候为了省事都是直接采用第三方的 Auth 包。这次使用 Gin 实现时决定参考去年写的 [JWT](/archives/sars2019-5.html) 文章实现一个简单的 token 认证，如果存在安全漏洞或者不周到的地方烦请指出。
<!-- more -->
　　思路很简单，服务器验证账号和密码后，将拼接的用户名与过期时间作为 `Payload`,使用 HMACSHA256 对拼接后的 `Payload` 进行签名。再与 Base64 编码后的 `Payload` 进行拼接。token 结构为 `SignaturePayload`.

```golang
// 设置签名 key 与超时时间
const secret = "Secretkey"
const exp = 31536000

// 生成 Token
func EncodeToken(username string) string {
    // 过期时间
    expireTime := time.Now().Unix() + exp
    // 拼接 payload
    data := username + "." + strconv.FormatInt(expireTime, 10)
    // base64 编码
    base64Data := base64.StdEncoding.EncodeToString([]byte(data))
    // 生成签名
	mac := hmac.New(sha256.New, []byte(secret))
	mac.Write([]byte(data))
    expectedMAC := hex.EncodeToString(mac.Sum(nil))
    // 生成 token
	token := expectedMAC + base64Data
	return token
}
```

　　用户之后的访问携带该 `token`，服务器将 Base64 编码的 `Payload` 解码，使用 HMACSHA256 签名并与 `Signature` 部分验证。如果通过验证，判断用户名是否存在和 `token` 是否过期。

```golang
func VerifyToken(token string, username *string) bool {
    // 提取签名与编码后的 payload
	messageMAC := token[:64]
    base64data := token[64:]
    // base64 解码
	data, err := base64.StdEncoding.DecodeString(base64data)
	if err != nil {
		return false
    }
    // 验证签名
	mac := hmac.New(sha256.New, []byte(secret))
	mac.Write(data)
	if messageMAC == hex.EncodeToString(mac.Sum(nil)) {
        // 验证用户名与超时时间
		*username = strings.Split(string(data), ".")[0]
		expireTime, _ := strconv.ParseInt(strings.Split(string(data), ".")[1], 10, 64)
		if controller.HavingUser(*username) && expireTime > time.Now().Unix() {
			return true
		}
		return false
	}
	return false
}
```