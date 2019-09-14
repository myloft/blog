---
title: Github Action + Hexo + COS 配置
date: 2019-09-14 19:00:00
updated: 2019-09-14 19:00:00
tags: hexo
---
　　Github Action 是 Github 推出的 CI/CD 工具，之前一直使用 Travis CI 进行部署，这次就尝试迁移到 Github Action 上，基本上属于换汤不换药。<!-- more -->

### 安装 hexo-deployer-cos
　　在 package.json 下的 dependencies 中添加：
```json
"hexo-deployer-cos": "^1.0.0"
```

### 配置 hexo-deployer-cos
　　在_config.yml 中填写配置信息。由于配置文件会上传到 github 中，为了安全文件不保存 SecretID 和 SecretKey。
```yml
deploy: 
  type: cos
  appId: 1234567890
  secretId: SecretId
  secretKey: SecretKey
  bucket: iloft-1234567890
  region: ap-shanghai
```
### 配置 Travis-CI
#### 配置环境变量
　　与使用 Travis CI 一样，我们首先在 `Repo/Setting/Secrets` 里配置需要环境变量。
![环境变量](/images/github-secrets.png)

#### 配置文件
　　创建并编辑 `.github/workflows/deploy.yml` ，与 Travis CI 格式大同小异，仍然需要使用 sed 命令替换隐私信息。由于Github Action支持私有仓库，如果是私有仓库可以直接在配置文件里填写敏感信息。
```yml
name: Deploy Hexo To COS & Github Pages

on:
  push:
    braches: 
      - master

jobs:
  build:
    runs-on: ubuntu-18.04

    steps:
    - uses: actions/checkout@v1
    - uses: actions/setup-node@v1
      with:
        node-version: '10.x'

    - name: Setup Hexo & COS env
      env:
        SecretId: ${{ secrets.SecretId }}
        SecretKey: ${{ secrets.SecretKey }}

      run: |
        npm install
        sed -i "s/SecretId/${SecretId}/" _config.yml
        sed -i "s/SecretKey/${SecretKey}/" _config.yml
    
    - name: Deploy to COS
      run: |
        hexo clean && hexo g && hexo d

    - name: Deploy to Github Pages
      env:
        GH_REF: github.com/myloft/blog
        CI_TOKEN: ${{ secrets.CI_TOKEN }}
        
      run: |
        git clone https://${GH_REF} .deploy_git
        cd .deploy_git
        git checkout master
        cd ../
        mv .deploy_git/.git/ ./public/
        cd ./public
        git init
        git config user.name "Solyn"
        git config user.email "admin@iloft.xyz"
        git add .
        git commit -m ":memo:\ Update blog by Actions"
        git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:gh-pages
```