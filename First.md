# 第一节 第一次的开服

#### 1.选择一个合适的服务端核心

目前国内主流的服务端大概有这样几种：

|核心名称|描述|指路|
| :------------ | :------------ | :------------ |
|官方服务端|无法添加插件，安装forge后可以添加mod|
|Bukkit/CraftBukkit|似乎是笔者印象中最为原始的插件服务端，并不支持添加MOD，但可以添加Bukkit插件|
|Spigot|Spigot对Bukkit进行了优化，不支持添加mod，但支持添加Bukkit插件|
|PaperSpigot|PaperSpigot对Spigot进行了优化，不支持添加mod，但支持添加Bukkit插件|https://papermc.io/|
|MCPC+/Cauldron/KC|可以安装Mod及Bukkit插件，但最大支持版本仅到1.7.10|
|Thermos|KC的优化版本|
|Uranium|KC的优化版本|
|Catserver|国人开发的1.12.2Mod+Bukkit插件服务端|https://www.mcbbs.net/thread-840599-1-1.html|
|Sponge|拥有自己的插件体系，在1.8及以上版本能做到Mod与Sponge插件的共同使用|
|Vanilla|支持Sponge插件但不支持Mod的轻型核心|

#### 2.添加启动脚本并运行

> 这里我们选用PaperSpigot1.12.2进行演示。
>
> 下载地址：https://papermc.io/
>
> 首先在我们的电脑上新建一个文件夹，建议纯英文路径。
>
> 将下载好的paper.jar扔进去。
>
![avatar](https://blog.drenal.cn/pictures/picture1.png)
> 在文件夹内创建一个server.bat，使用文本编辑器（如记事本，Notepad++）打开。
>
> 输入最基本的启动代码

  ```bash
@ECHO OFF
java -Xmx1G -Xms1G -jar paper.jar
pause
```

> 保存并运行（可能需要挂梯子）
>
> 找到文件夹中的eula.properties文件.
>
![avatar](https://blog.drenal.cn/pictures/picture2.png)
>
> 将其中的eula=false改为eula=true;
> 
> 重新启动start.bat;
>
> 我们发现服务器现在已经可以成功启动了。
>
![avatar](https://blog.drenal.cn/pictures/picture3.png)
> 在终端窗口中输入stop停止运行。
>
> 在根目录中找到server.properties文件。
>
> 我们可以根据情况修改几个重要内容。
>
![avatar](https://blog.drenal.cn/pictures/picture4.png)
#### 3.对服务器进行一些配置
> 开放刚刚在server.properties中填写的端口;
> 
> 打开服务器管理器。
>
> 找到工具->高级安全Windows防火墙->入站规则。
>
> 新建规则，选择端口。
>
> 下一步。
>
> TCP，特定本地端口，输入你想要开放的端口。
>
> 下一步。
>
> 允许连接。
>
> 下一步怼到底。
>
> 至此，您应该能通过您服务器的 「IP地址:端口」 来连接您的服务器了。
>