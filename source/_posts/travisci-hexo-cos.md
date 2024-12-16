---
title: Travis CI + Hexo + COS 配置
date: 2019-06-20 16:30:00
updated: 2019-06-20 16:30:00
tags: hexo
---
　　之前一直使用Travis CI + Github Pages + Hexo + Cloudflare的方案托管博客，省心速度还将就。最近一段时间CF的443端口出现劣化现象，于是将博客重新迁移回腾讯云对象存储上了。并记录一下配置。
<!-- more --> 

安装hexo-deployer-cos
---
　　在package.json下的dependencies中添加：
```json
"hexo-deployer-cos": "^1.0.0"
```

配置hexo-deployer-cos
---
　　在_config.yml中填写配置信息。由于配置文件会上传到github中，为了安全文件不保存SecretID和SecretKey。
```yml
deploy: 
  type: cos
  appId: 1234567890
  secretId: SecretId
  secretKey: SecretKey
  bucket: iloft-1234567890
  region: ap-shanghai
```
配置Travis-CI
---
　　在Travis-CI项目设置界面中配置环境变量。
![配置环境变量](/images/travis-environment-variables.jpg)  
　　修改.travis.yml，与只部署到Github Pages相比，增加了将SecretId与SecretKey替换成为预设的环境变量的sed命令和hexo d命令。完整的部署到COS和Github Pages配置文件：
```yml
language: node_js
node_js: stable
cache:
  directories:
    - node_modules

# S: Build Lifecycle
install:
  - npm install


#before_script:
 # - npm install -g gulp

script:
  - sed -i "s/SecretId/${SecretId}/" _config.yml
  - sed -i "s/SecretKey/${SecretKey}/" _config.yml
  - hexo clean && hexo g && hexo d

after_script:
  - git clone https://${GH_REF} .deploy_git
  - cd .deploy_git
  - git checkout master
  - cd ../
  - mv .deploy_git/.git/ ./public/
  - cd ./public
  - git init
  - git config user.name "Yu"
  - git config user.email "admin@iloft.xyz"
  - git add .
  - git commit -m ":memo:\ Update blog by CI"
  - git push --force --quiet "https://${CI_TOKEN}@${GH_REF}" master:gh-pages
# E: Build LifeCycle

branches:
  only:
    - master
env:
 global:
   - GH_REF: github.com/myloft/blog
```