# Mac OS 技巧

## 软件卸载了图标还在launchpad上怎么办

```shell
defaults write com.apple.dock ResetLaunchPad -bool true; killall Dock
```

## 如何隐藏MacOS命令行终端的电脑名和用户名信息

> Terminal -> Settings -> Profiles -> Shell

```textile
PS1="%~ $ "; clear;
```

![](https://raw.githubusercontent.com/0x2552/images-repo/main/2023/03/31-23-04-57-2023-03-31-23-04-53-image.png)

![](https://raw.githubusercontent.com/0x2552/images-repo/main/2023/03/31-23-06-00-2023-03-31-23-05-54-image.png)

## MacOS 环境变量规则

Mac存在多种设置环境变量的方式，根据加载的时机和范围不同，分为不同的文件，默认使用zsh。

MAC OS X环境的所有配置以及加载顺序如下：

```shell
# 系统级别
/etc/profile
/etc/paths 

# 用户级别
~/.bash_profile 
~/.bash_login 
~/.profile 

~/.bashrc（或者~/.zshrc）
```

- 前两个环境配置在系统启动时候就会加载，针对所有用户生效，后面四个属于具体用户级别的配置

- ~/.bash_profile，~/.bash_login，~/.profile依次加载，如果~/.bash_profile不存在，依次加载后面几个文件；如果~/.bash_profile文件存在，后面几个文件不会加载

- ~/.bashrc （或者~/.zshrc ）是bash shell打开时候加载

- ~/.bashrc （或者~/.zshrc）的区别，zsh终端命令工具的全局变量设置，和bashrc区别是 默认很多linux系统是base，就配置在bashrc里，如里是使用zsh 就配置在 zshrc里，zsh是比bash更强大shell



## 显示隐藏文件

```shell
command + shift + .
```
