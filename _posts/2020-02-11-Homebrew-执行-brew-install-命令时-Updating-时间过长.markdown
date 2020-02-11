---
layout: post
title:  "Homebrew 执行 brew install 命令时 Updating Homebrew 时间过长"
author: "彭淜"
email:  "bexon@foxmail.com"
date:   "2020-02-11"
lang:   "zh-CN"
categories: ["Web", "Mac"]
excerpt: "Homebrew 是一款 macOS, Linux 平台下的软件包管理工具，使用它可以非常轻松地安装、卸载、更新、查看、搜索软件包。本文中将使用阿里巴巴的镜像解决在安装软件时，由于国内网络不稳定而造成的 Updating Homebrew 过程时间过长"
---
由于 Homebrew[^1] 官方使用的是 GitHub 来做源，但 GitHub 即使归于微软公司麾下了，在国内的网络环境下还是非常不稳定，所以在使用 Homebrew 安装软件包的时可能会长时间卡在 Updating Homebrew... 这个步骤，如下。
```bash
➜ ~ brew install imagemagick                                              
Updating Homebrew...
```

在 brew 执行时，主要使用到3个源，以下為阿里的源：
- https://mirrors.aliyun.com/homebrew/brew.git
- https://mirrors.aliyun.com/homebrew/homebrew-core.git
- https://mirrors.aliyun.com/homebrew/homebrew-bottles

废话不多说，直接开始👨‍💻！

## 1. 切换 brew.git 的仓库地址
```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
```
如需换回官方的仓库地址
```bash
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```

## 2. 切换 homebrew-core.git 的仓库地址
```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.aliyun.com/homebrew/homebrew-core.git
```
如需换回官方的仓库地址
```bash
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```

## 3. 切换 homebrew-bottles 访问地址
这个步骤跟系统使用的 shell 版本有关系

所以，先来查看当前使用的 shell 版本

```bash
echo $SHELL

# 如果你的输出结果是 /bin/zsh，参考下方的 zsh 终端操作方式
# 如果你的输出结果是 /bin/bash，参考下方的 bash 终端操作方式
```

### 3.1 zsh (Oh My Zsh) 终端操作方式
```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.zshrc
source ~/.zshrc

#=======================================================

# 如需还原為官方提供的 homebrew-bottles 访问地址
open -e ~/.zshrc
# 然后删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
source ~/.zshrc
```

### 3.2 bash 终端操作方式
```bash
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
 
#=======================================================
 
# 如需还原為官方提供的 homebrew-bottles 访问地址
open -e ~/.bash_profile
# 然后删除 HOMEBREW_BOTTLE_DOMAIN 这一行配置
source ~/.bash_profile
```

🍺  Done!

[^1]: 适用于 macOS, Linux 的缺失包管理器