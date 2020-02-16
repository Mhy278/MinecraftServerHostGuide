# 第一节 Redis 的安装与配置

## 简介
Redis（全称：Remote Dictionary Server 远程字典服务）是一个开源的使用ANSI C语言编写、支持网络、可基于内存亦可持久化的日志型、Key-Value数据库，并提供多种语言的API。
- Redis 是完全开源免费的，遵守BSD协议，是一个高性能的 key-value 数据库。
- Redis 与其他 key-value 缓存产品有以下三个特点：
- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的 key-value 类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即 master-slave 模式的数据备份。
## 优势
- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。


## 下载
### Window 系统

下载地址：https://github.com/MSOpenTech/redis/releases

### Linux 系统

下载地址：https://redis.io/download

**注：Redis官方并无Windows支持，目前Redis主版本已到5.X，而Windows下的最高版本仍为3.2.100。**

## Msi安装模式
若想采用此模式则应在上方下载地址中选择下载[Redis-x64-3.2.100.msi](https://github.com/microsoftarchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.msi)
- 通过此模式安装我们可以对Redis进行简易的安装配置
- 首先双击打开Redis-x64-3.2.100.msi，等待文件读取完成即可点击NEXT按钮
- 点击NEXT按钮后需要同意软件使用协议，选中I accept the terms in the Licenes Agreement复选框即可再次点击NEXT按钮
- 选择软件的安装目录，如不了解此选项的具体含义请直接点击NEXT按钮进入下一步
- 设置Redis服务的运行端口，默认为6379，如不了解此选项的具体含义请直接点击NEXT按钮进入下一步
- 设置Redis服务的运行内存限制，建议为服务器总内存的5%-10%，如不了解此选项的具体含义请直接点击NEXT按钮进入下一步
- 在这一步直接点击Install按钮即可进入Redis的安装过程
- 等待进度条完成后点击Finish按钮即可完成安装，此时Redis的系统服务也已自动启动，现在只需要在相关的插件配置文件中配置好Redis的链接信息即可使用，请注意，在您未更改Redis默认配置的情况下，Redis的默认链接信息为：
Host:127.0.0.1
Port:6379
Pass:默认为空


## 解压可用模式
若想采用此模式则应在上方下载地址中选择下载[Redis-x64-3.2.100.zip](https://github.com/microsoftarchive/redis/releases/download/win-3.2.100/Redis-x64-3.2.100.zip)
- 首先我们将下载的zip压缩文件解压至任意文件夹,进入文件夹
- 双击redis-server.exe文件启动Redis服务端
注：该模式下默认配置为：
Host:127.0.0.1
Port:6379
Pass:默认为空

## Redis配置
一般来说Redis中我们经常用到的配置项有以下几项：
- `port:6379`指定访问redis服务端的端口。
- `bind:127.0.0.1`指定redis绑定的主机地址。
- `timeout:`指定客户端连接redis服务器时，当闲置的时间为多少（如300）时，关闭连接。
- `loglevel:`指定redis数据库的日志级别，常用的日志级别有`debug`、`verbose`、`notice`、`warning`，不进行修改的情况下默认的是`notice`。
- `save:`指定redis数据库多长时间内（s）有多少次（c）更新操作时就把缓存中的数据同步到本地库，比如：save 600 2，指的是10分钟内有2次更新操作，就同步到本地库。
- `dir:`指定redis本地数据文件存放的目录。
- `requirepass:`指定redis的访问密码。
注：如果您无法理解上面每一项的含义，那么请勿修改配置文件

## RedisBungee插件配置
这里仅展示Redis相关部分：
```
#Redis服务器IP，默认为127.0.0.1
redis-server: 127.0.0.1
#Redis端口，默认为6379
redis-port: 6379
#Redis密码,默认为空
#如果你的Redis密码为 纯数字 请删除 ""
redis-password: ""
#Redis服务器最大连接数.
#默认值为 8. 请不要改变这个值,除非你有大量插件或玩家.
max-redis-connections: 8
```
插件详细配置请前往MCBBS查看[RedisBungee](https://www.mcbbs.net/thread-691147-1-1.html)的介绍。
