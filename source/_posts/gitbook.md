---
title: Gitbook搭建
date: 2017-01-14 22:41:05
tags: [doc]
categories: 工具
---
> 平时写一个完整的文档，一般推荐最多的是[Gitbook](https://www.gitbook.com/)，本篇借 [LearnGraphQL-CN](https://github.com/graphqlhelp/LearnGraphQL-CN) 的迁移来记录 `Gitbook` 使用的过程。

<!-- more -->

Gitbook的官方描述是 `Modern book format and toolchain using git and markdown.`总体使用的感觉是: 界面不错、比较方便，但是毕竟是国外的服务（你懂得）...

下面来介绍具体的搭建过程:

# [gitbook-cli](https://github.com/GitbookIO/gitbook-cli)安装
## 常用命令
```shell
# 安装gitbook命令行工具: gitbook-cli
npm install -g gitbook-cli

# 初始化目录
## 生成`README.md`和`SUMMARY.md`: 书籍目录结构
gitbook init <prj-name>

# 本地起服务
## 内容修改会自动刷新
gitbook serve
```

## 其他命令
- `help`: 所有可用的命令
- `ls`: 查看安装的版本
- `ls-remote`: 查看远端可用的版本
- `fetch 2.1.0`: 安装特定版本
- `update`: 更新
- `uninstall 2.1.0`: 卸载

# 部署与发布
> 基于 `gitbook`（发行书籍的社区） 和 `github`（代码托管，方便fork）。

- 在github上创建文档仓库

- 对gitbook进行github的授权
    + Account setting 中 link Github
    + Ps: 如果授权在新建仓库之前，需要点击一下`Reconnect Github Account(with access to public reponsitory)`

- 在gitbook上create a new book
    + 选择: BOOK & MANUAL

- 在新建的book上点击setting
    + 选择Github仓库
    + 增加Webhook（可自动、可手动）
    + 完成后会显示 `IN SYNC`

- 之后在Github上推送代码，即可实现自动发布
```

额...好像这篇文章比较短，但是已经够用了，不过对于Gitbook，其实还有很多扩展性的插件，此处先留个TODO:哈，后续有时间再讲...撒油啦啦...
