# 第二节 服务端的结构

## 前言

- 根据上节的内容，您应该已经可以成功启动您的服务器了。本节内容将继续带您简单了解服务端的结构。
- 由于 Sponge 服务端不太适合新手使用，故不做介绍，如有需要请自行查看[官方教程](https://docs.spongepowered.org/stable/zh-CN/server/quickstart.html)（含中文）。

## 服务端结构概览

- 以下是两种基于 Bukkit 核心的服务端。

我们先来简单看一眼 Paper 的文件夹结构：
```c
Paper
├─cache //缓存文件夹
├─logs //存放服务端日志的文件夹
├─plugins //存放插件的文件夹
│  └─bStats //一些插件生成的文件夹，一般情况下存放着插件的配置文件
├─world //世界文件夹
│  ├─data //世界数据文件夹
│  │  ├─advancements
│  │  └─functions
│  ├─playerdata //存放了玩家数据
│  └─region
├─world_nether //地狱世界文件夹
│  ├─data
│  │  └─functions
│  ├─DIM-1
│  │  └─region
│  └─playerdata
└─world_the_end //末地世界文件夹
    ├─data
    │  └─functions
    ├─DIM1
    │  └─region
    └─playerdata
```
以及 Catserver 的文件夹结构：
```c
Catserver
├─config //模组的一些设置
├─libraries //一些支持库
├─logs //存放服务端日志的文件夹
├─mods //存放模组的文件夹
├─plugins //存放插件的文件夹
│  └─bStats //存放插件配置的一些文件夹
└─world //世界文件夹
    ├─data //世界数据文件夹
    │  ├─advancements
    │  └─functions
    ├─DIM-1 //世界里的世界#滑稽
    │  ├─data
    │  │  └─functions
    │  ├─playerdata
    │  └─region
    ├─DIM1
    │  ├─data
    │  │  └─functions
    │  ├─playerdata
    │  └─region
    ├─playerdata //玩家数据文件夹
    └─region
```

## 根目录下的一些文件

- `banned-ips.json`

存放了被 ban IP 玩家的 UUID 及游戏名称等相关信息。

- `banned-players.json`

存放了被 ban ID 玩家的 UUID 及游戏名称等相关信息。

- `bukkit.yml`

由 Bukkit 派生得到的服务端都有的文件。保存着 Bukkit 所提供的最基本的设置选项。

-  `commands.yml`

它提供了一种快速而简单的方法来强制服务器使用Mojang提供的命令（而不是Bukkit中内置的命令）而不产生冲突。

- `eula.txt`

在首次开服的时候，我们需要同意 EULA 协议。

- `help.yml`

一些插件的帮助信息会出现在这里面，一般不需要进行什么修改。

- `ops.json`

op们的游戏昵称 UUID 等信息会在这里出现。

- `paper.jar`

这个叫什么名字无所谓，重要的是他的本质工作就是安安稳稳地当一个服务端核心。

- `paper.yml`

这是 Paper 才有的配置文件，您一般不会在 Bukkit 或 Spigot 中看到它。

- `permissions.yml`

是默认的权限定义YAML文件，在启动时自动生成。它的主要功能是将权限节点组合到一个文件中，以便权限插件（以及使用权限的插件）可以在整个系统中使用它。

- [`server.properties`](https://minecraft-zh.gamepedia.com/Server.properties)

服务端的一些基本设置就在里面，如设置端口，设置游戏难度，设置游戏模式（生存、创造等）等。

- `spigot.yml`

这是 Spigot 才有的配置文件，您一般不会在 Bukkit 中看到它。因为 Paper 是由 Spigot 派生而来的服务端，所以在 Paper 的服务端中，它也会出现。

- `usercache.json`

储存着用户部分信息的缓存文件。

- `version_history`

一些 Minecraft 和服务端核心的版本信息。

- `whitelist.json`

白名单列表。（如果您在[`server.properties`](https://minecraft-zh.gamepedia.com/Server.properties)中开启了白名单，您就可以在该文件中添加和管理谁被允许或不被允许进入服务器。）

至此，大部分的以 Bukkit 为基础的服务器核心服务端的结构已经大致呈现在您眼前。
