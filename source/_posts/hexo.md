---
title: 基于 Hexo 的博客搭建
date: 2017-01-03 02:23:06
tags: [Hexo]
categories: 工具
---
> 开坑第一篇，谈谈 Hexo 博客的搭建...

<!-- more -->

主要内容如下：
- 选型
- 仓库搭建
- 基本安装
- 目录结构
- 日常使用

## 选型
由于 Hexo 基于 `Node`（Jekyll基于ruby，wp太...），然而不会 `Node` 的前端不是好司机，选择 Hexo 的主要原因是想借次入门 `Node`。

然后，[官方](https://hexo.io/zh-cn/) 还介绍了它的特性:
- 速度超快: 主要指生成页面和渲染页面的速度
- 支持Markdown: 涵盖[ GitHub Flavored Markdown ](https://guides.github.com/features/mastering-markdown/)的所有功能以及`Octopress`的插件
- 一键部署: 一个命令就可以部署到各个平台【👍】
- 插件丰富: 如支持jade等..

## 仓库搭建
由于 Hexo 的部署命令 `hexo d` 是利用 hexo-deployer-git 来实现 git 推送的，其原理是通过在站点中生成 `.deploy_git` 文件夹，通过读取相关配置将其 push 到远程仓库（如果不存在会用git init）。而其中 `.deploy_git` 进包含生成的静态博客（source等源文件），因此我们需要[采用分支](https://www.zhihu.com/question/21193762)进行博客管理。

所以，在github和coding上分别新建 zchar-hong.github.io 和 zchar（如果与用户名相同，则博客地址为 http://zchar.coding.me/ ），并分别开启 Page 功能（github在setting里，coding在代码里）。

### SSH配置
出于不想重复输入用户名、密码以及单次上传限制的问题，在使用 [Git协议](https://coding.net/help/doc/git/ssh-key.html) 上，决定不采用HTTPS（加密的网页访问通道），而是采用 SSH（加密通道），它使用还算方便，只需要完成公钥配对即可。

公钥生成: `ssh-keygen -t rsa -C "任意的注释"`
-t: 是创建秘钥的类型
-C: 注释（生成秘钥和邮箱没有关系，如果觉得麻烦其实可以公用一个 id_rsa.pub）

测试是否成功:
ssh -T git@git.coding.net

Ps: 可以针对不同的服务生成不同的公钥（如: `id_ras_github.pub`），此处注意不要重名即可（<font color=red>重名会被覆盖</font>），如果觉得麻烦，其实也可以使用一个公钥（`id_ras.pub`），但是建议在生成公钥时，对私钥设置密码（当私钥不小心泄露时，别人拷贝后，还需要输入密码），这样会相对安全一点。

## 环境与初始化
```bash
# 环境
## 1. node + git

## 2. 安装hexo
npm i -g hexo-cli

# 初始化
## 1. hexo init 会把 .git 干掉，所以只能先hexo init，再关联远程仓库。
hexo init zchar-hong.github.io
cd zchar-hong.github.io
npm i

## 2. 安装hexo-deployer-git，用于发布站点
npm i -S hexo-deployer-git

## 3. 拉源码分支
git checkout -b hexo

## 4. 提一版
git add -A
git commit -m 'first ci'

## 5. 关联远程仓库
git remote add origin git@github.com:zchar-hong/zchar-hong.github.io.git

## 6. 设置hexo为源码push流
git push -u origin hexo

## ---- 后续怎么玩都行 ----

## 7. 修改_config.yml的部署配置
# Site
title: Huang Zhichao
subtitle: 前端码农
description:
author: ZCHAR
language: zh-Hans
timezone: Asia/Shanghai

deploy:
  type: git
  repo:
    github: git@github.com:zchar-hong/zchar-hong.github.io.git,master
    coding: git@git.coding.net:zchar/zchar.git,master
  name: huangzhichao
  email: zchar.hong@qq.com
  ignore_hidden: true

## 8. 换主题
### 官方主题: https://hexo.io/themes/
### next: https://github.com/hexojs/hexo-deployer-git
### 8.1 submodule
git submodule add https://github.com/iissnan/hexo-theme-next themes/next
### 8.2 指定主题（config.yml）
theme: next
### 8.3 主题配置: (http://theme-next.iissnan.com/)
- avator: 替换为自己的头像
- scheme: Pisces 主题
- baidu_analytics: 添加统计代码即可
### 注意: 在项目中theme拉下来是git仓库（为了随时获取next主题的更新），为了可以自己维护，建议自己fork，然后提到自己的库，此处使用submodule
### git submodule add remote-url theme/next
### 再次克隆的时候，需要git submodule init进行注册

## 9. 默认是没有的，创建分类和标签页
hexo new page 'tags'
hexo new page 'categories'
### 主题的config.yml
menu:
  home: /
  tags: /tags
  categories: /categories
```

## 目录结构
- _config.yml: 站点配置，关于`yml`详见扩展。
- package.json
  - [EJS](http://www.embeddedjs.com/)
  - stylus
  - Markdown render
- scaffolds: 模板文件
  - draft: todo，如何用？
  - page
  - post

## [日常使用](https://hexo.io/zh-cn/docs/configuration.html)
- 新建文章: hexo new title
- 清缓存: hexo clean
- 起服务: hexo s [-p 端口]
- 部署: hexo d

## 扩展文章
### [YML语言教程](http://www.ruanyifeng.com/blog/2016/07/yaml.html?f=tt)
> yml是专门用来写配置的语言，比JSON简洁。

#### 语法特点:
- 大小敏感
- 缩进表层级
- 不允许用tab，仅允许用空格
- 对空格数无要求，同级左对齐即可
- `#` 表示注释

#### 数据结构:
- 对象: 简写
- 数组
- 纯量（scalars）: 不可再分的值
  + 字符串: !!str 123（强制类型转化）
  + 布尔值
  + 整数
  + 浮点数
  + Null: `~` 表示
  + 时间
  + 日期

```js
// 1. 对象简写: animal: pets
// 自动加 {} 和 对值加 ''
{
  animal: 'pets'
}

// 2. 数组
- Cat
- Dog
- Goldfish
// 转为
[ 'Cat', 'Dog', 'Goldfish' ]

// 行内表示
animal: [Cat, Dog]
// 转为
{ animal: [ 'Cat', 'Dog' ] }

```
#### 引用
- &: 锚点
- *: 别名
- <<: 合并
```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```

## TODO
还有其他的使用，后续持续更新。

