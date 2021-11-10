---
title: Hexo博客通过Github Actions实现CI
date: 2021-11-08 19:00:49
cover: https://img.showydream.com/img/bnNPUN-k-mitch-hodge-wE7T6f1GlxA-unsplash.jpg
description: Hexo博客通过github Actions实现CI
keywords: Hexo, GitHub Actions
tags: Hexo
categories:
  - Hexo
---

通过Hexo搭建的个人博客，虽然很强大、很方便，输入命令就可以快速生成静态页面并部署访问，但是还有让它更好用的优化空间，比如直接把打包部署环境放到`Github Actions`上面，我个人就是用这种形式部署博客的。下面来介绍一下。

## 总体思路

- Hexo项目里配置好Github Actions流程
- Hexo博客项目上传到GIthub
- Github Actions自动打包并把打包文件上传到腾讯云服务器
- 腾讯云服务器替换Nginx的入口文件为新的打包文件

## 功能实现

### 创建配置

在项目根目录创建 `.github/workflows/{workflow-name}.yml` 文件。这里的`workflow-name`根据根据不同的 event 或者要实现的 actions 来创建。

### 项目配置

根据注释，修改自己的配置

```yaml
name: main

# 触发条件：push到main分支后
on:
  push:
    branches:
      - main

# 任务
jobs:
  #job1
  build_and_deploy:
    name: build-and-del=ploy
    runs-on: ubuntu-latest

    steps:
      # 拉取代码
      - name: checkout
        uses: actions/checkout@v2

      # 生成静态文件
      - name: Use Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "14.x"
      - run: npm install hexo-cli -g
      - run: npm install && npm run build
      - name: Deploy to GitHub Pages
        uses: crazy-max/ghaction-github-pages@v2
        with:
          target_branch: gh-pages
          build_dir: public
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      # 自动部署到腾讯云服务器
      - name: deploy to tencent
        uses: easingthemes/ssh-deploy@v2.1.5
        env:
          # 私钥
          SSH_PRIVATE_KEY: ${{ secrets.SERVER_SSH_KEY }}
          ARGS: "-rltgoDzvO --delete"
          # 源目录，编译后生成的文件目录
          SOURCE: public
          # 服务器ip：换成你的服务器IP
          REMOTE_HOST: ${{ secrets.SERVER_IP }}
          # 用户
          REMOTE_USER: root
          # 目标地址 你在服务器上部署代码的地方
          TARGET: /workspaces
```

### Secrets

在github控制面板中添加以下Secrets：

- GITHUB_TOKEN - 用于发布 Github Release
- SERVER_IP - 腾讯云服务器外网IP
- SERVER_SSH_KEY - 腾讯云SSH私钥

## 完成

在项目main分支push代码即可触发工作流程，出现绿色勾勾则表示部署成功

<img src="https://img.showydream.com/img/tzClJy-image-20211108193931702.png" alt="image-20211108193931702" style="zoom:50%;" />

点进去可以看到一次触发的流程，我这里只有一个流程

<img src="https://img.showydream.com/img/ozs5tM-image-20211108194114615.png" alt="image-20211108194114615" style="zoom:50%;" />

再点进去可以看到详细的部署日志

<img src="https://img.showydream.com/img/PfrKoo-image-20211108194150801.png" alt="image-20211108194150801" style="zoom:50%;" />

## 总结

我这个还不算完全CI，因为在腾讯云服务器我是用宝塔建的站，文件直接传上去不能直接覆盖nginx的入口文件，需要在服务器再操作一下。留个坑，以后填



