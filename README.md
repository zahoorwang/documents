[![Build Status](https://travis-ci.org/shanyuhai123/documents.svg?branch=master)](https://travis-ci.org/shanyuhai123/documents)

## 介绍
这是一个 VuePress 搭建的文档
具体如何搭建 VuePress 文档可以通过我录制的视频来学习, 视频地址为 https://www.bilibili.com/video/av43316513/

## 如何使用
```bash
# 1. 首先拷贝该项目
git clone git@github.com:shanyuhai123/documents.git

# 2. 安装依赖
yarn add # 或者 npm i

# 3. 本地运行查看(在运行前请先进行相关配置)
yarn docs:dev # 或者 npm run docs:dev

# 4. 推送到 github 的 gh 分支
yarn deploy # 或者 npm run deploy
```

## 说明

最近将评论模块删除了，因为觉得文档属于更加个人的东西，如果真的有需要自然会有在 Issue 中留言的。
关于隐私的一些配置，也开放出来了，当然我是强烈不推荐这种行为，之后会进一步思考如何和 Travis CI 来进行配置避免这种行为。