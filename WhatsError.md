# 第四节 什么是报错？

**报错**是服务器运行中所遇到的异常情况，它可以来源于**服务端本身**，也可以来自**插件**和**模组**等组件，可能是**你的锅**也可能是**开发者的锅**，这两种情况下面会分别考虑。
+ **插件报错**往往导致该插件和它的附属插件**无法正常运行**，部分情况下，**玩家的信息会流失**，玩家也可能通过这些错误来**破坏游戏平衡**,极少部分情况下，**服务器会崩溃**。
+ **模组报错**是更为致命的，大多数情况下会让**服务器直接崩溃**，这可能会导致**玩家数据和地图数据流失损坏**。(某位不知名OP也深受其害...  
可幸的是一些模组的报错出现在服务器**启动完成之前**，不会留下隐患。

**很多服主在面对不影响正常体验的报错时采取放任的态度，大部分时候确实相安无事，但解决错误应该作为最优先的策略**


当服务器运行出错的时候，会在后台打印出如下类似的`ERROR信息`。它的标志是信息前显眼的 **ERROR**。

这段报错来自装错版本的Citizens插件

	[17:36:48 ERROR]: [Citizens] Missing preferred location bundle.
	[17:36:48 ERROR]: Error occurred while enabling Citizens v2.0.16-SNAPSHOT (build 1267) (Is it up to date?)
	
这段报错来自残忍怪物模组
    
	[Server thread/ERROR]: Problem editing navigator: java.lang.NoSuchFieldException: navigator
	
这是Protocol插件的版本警告。WARN级别的警告信息有时候也需要服主留意。

	[14:11:28] [Server thread/WARN]: [ProtocolLib]
	Version (MC: 1.12.2) has not yet been tested! Proceed with caution.
    

 大多数情况下,ERROR 级别的报错比 WARN 更严重。但是留 心 WARN 警告的好习惯可以让我们注意到潜在的问题。
 
 Error日志会报告服务器运行过程中的异常情况，这些情况有可能**破坏服务器的正常运行**，导致**玩家体验恶化，信息损坏，甚至服务器崩溃**。

---

## 如何自己解决报错？
很多时候，一些常见的报错可以用常识、经验以及一定的英语能力(或借助翻译软件)来解决。
### 一. 找出问题关键信息

报错会产生海量的信息，然而绝大多数内容都无关紧要，去伪存真才能对症下药。这里再次用上文的 Cizitens 的版本报错为例，但会给出完整版,感兴趣的服主可以借此研究报错的结构，但这并不是必须的。

    [17:36:48 ERROR]: [Citizens] Missing preferred location bundle.
    [17:36:48 ERROR]: Error occurred while enabling Citizens v2.0.16-SNAPSHOT (build 1267) (Is it up to date?)
    java.lang.NoClassDefFoundError: org/bukkit/craftbukkit/v1_8_R3/CraftServer
        at net.citizensnpcs.util.Util.getMinecraftRevision(Util.java:78) ~[?:?]
        at net.citizensnpcs.Citizens.onEnable(Citizens.java:262) ~[?:?]
        at org.bukkit.plugin.java.JavaPlugin.setEnabled(JavaPlugin.java:264) ~[spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.plugin.java.JavaPluginLoader.enablePlugin(JavaPluginLoader.java:337) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.plugin.SimplePluginManager.enablePlugin(SimplePluginManager.java:402) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.craftbukkit.v1_12_R1.CraftServer.enablePlugin(CraftServer.java:374) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.craftbukkit.v1_12_R1.CraftServer.enablePlugins(CraftServer.java:323) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.craftbukkit.v1_12_R1.CraftServer.reload(CraftServer.java:745) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.Bukkit.reload(Bukkit.java:525) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.command.defaults.ReloadCommand.execute(ReloadCommand.java:25) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.command.SimpleCommandMap.dispatch(SimpleCommandMap.java:141) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.craftbukkit.v1_12_R1.CraftServer.dispatchCommand(CraftServer.java:641) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.craftbukkit.v1_12_R1.CraftServer.dispatchServerCommand(CraftServer.java:627) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at net.minecraft.server.v1_12_R1.DedicatedServer.aP(DedicatedServer.java:444) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at net.minecraft.server.v1_12_R1.DedicatedServer.D(DedicatedServer.java:407) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at net.minecraft.server.v1_12_R1.MinecraftServer.C(MinecraftServer.java:679) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at net.minecraft.server.v1_12_R1.MinecraftServer.run(MinecraftServer.java:577) [spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at java.lang.Thread.run(Unknown Source) [?:1.8.0_231]Caused by: java.lang.ClassNotFoundException: org.bukkit.craftbukkit.v1_8_R3.CraftServer
        at org.bukkit.plugin.java.PluginClassLoader.findClass(PluginClassLoader.java:91) ~[spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at org.bukkit.plugin.java.PluginClassLoader.findClass(PluginClassLoader.java:86) ~[spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
        at java.lang.ClassLoader.loadClass(Unknown Source) ~[?:1.8.0_231]
        at java.lang.ClassLoader.loadClass(Unknown Source) ~[?:1.8.0_231]
        ... 18 more

我们可以看到报错的最后出现了一系列由at引导的语句。他们的作用是**定位出错的代码位置**。  
**当 at 语句结束，下面仍然有其他的 Error 报错时，说明出现了两个以上个不同的报错，请以最后一个 at 为分界线，把他们作为两个独立的报错分析，以此类推**  
  
  
  
at 的位置越靠后，他们和报错的关系就越疏远。所以我们需要设定一个分界线语句，删去分界线后的 at，以此简化信息。(请保留分界线所在的语句)  
  
### 可以作为分界线的语句  
① 以`at java./sun./org.apache.`等开头的语句，他们是 java 语言的内部包  
② 以`at org.bukkit.craftbukkit`或 `at net.minecraft.server` 开头的语句，他们是服务器的内部包  
  
通过以上的教程自己动手后，我们可以得到如下简化后的报错  

    [17:36:48 ERROR]: [Citizens] Missing preferred location bundle.
    [17:36:48 ERROR]: Error occurred while enabling Citizens v2.0.16-SNAPSHOT (build 1267) (Is it up to date?)
    java.lang.NoClassDefFoundError: org/bukkit/craftbukkit/v1_8_R3/CraftServer
        at net.citizensnpcs.util.Util.getMinecraftRevision(Util.java:78) ~[?:?]
        at net.citizensnpcs.Citizens.onEnable(Citizens.java:262) ~[?:?]
        at org.bukkit.plugin.java.JavaPlugin.setEnabled(JavaPlugin.java:264) ~[spigot-1.12.1.jar:git-Spigot-edd0396-2a927e8]
    
这样的报错就可以用来分析了。请注意，以上使用插件报错作为样例，对于模组报错理论上有效，但可能会有形式上的变化。  

美丽小贴士：**精简报错的目的是方便自己分析**，如果相信自己的水平，就不需要进行这个过程。   


---
## 二. 问题来自哪个插件/模组？  
对于  

    [17:36:48 ERROR]: Error occurred while enabling Citizens
这样的报错，我们可以很轻松地看出问题发生在Citizens插件上。  
  
而对于

    [13:39:13] [Server thread/ERROR]: Could not pass event EntityTeleportEvent to PlotSquared

`Could not pass event <Event名称> to <插件名称>`也是一种常见的报错形式。  
在这种报错中，我们也可以定位到具体的插件。这里的Event名称可以保存下来，我们会在下面内容中谈到。   

**以下内容同时适用于插件和模组：**

并非所有报错都有这样详细的形式。这时候,我们需要取出最上方的at语句，我们再次请出样例Citizens插件：

    at net.citizensnpcs.util.Util.getMinecraftRevision(Util.java:78)~[?~?]
这段报错对于寻找问题插件/模组，也是一目了然的。毕竟文本里已经出现了插件/模组名  

如果这样的分析依然无法得知具体插件,建议将这个问题发送到mcbbs问答板块解决。  

到了这个地步，服主仍然希望自己找到报错的插件/模组。我们就只好取出at后面的一小段内容，如`net.citizensnpcs.util`,将他改为`net/ciizensnpcs/util`,然后用压缩软件打开每个插件/模组（插件/模组的本质是压缩文件），寻找对应的文件夹。如果能寻找到对应的文件夹或文件，就说明问题出现在这个插件/模组里。  
  
服主也可以通过安装某些服务器管理插件来帮助寻找，但是这里不作安利。

---
## 三. 分析问题的方法

### 1.插件提示法  
一些插件会在报错信息中给出具体的提示,可以借助英语知识及翻译软件帮助以求解决。  
例如：

    [14:48:03 ERROR]: [PlaceholderAPI] GameTime is currently using a deprecated method to hook into PlaceholderAPI. 
    Placeholders for that plugin no longer work. Please consult [a, asougi85] and urge them to update it ASAP.

翻译： [GameTime](https://www.mcbbs.net/thread-714090-1-1.html) 插件正使用一种过时的方法与PAPI联动，现在该功能已经无法使用，请联系作者。  
一目了然，看来这位不知名开发者有必要进行版本升级了。

### 2.Expcetion/Error法  
在大多数报错中，我们都可以看到在 at 之前看到这样的语句  

    [17:36:48 ERROR]: Error occurred while enabling Citizens v2.0.16-SNAPSHOT (build 1267) (Is it up to date?)
    java.lang.NoClassDefFoundError: org/bukkit/craftbukkit/v1_8_R3/CraftServer/...

  
    [14:48:02 ERROR]: Error occurred while enabling Multiverse-Core v2.5-b641 (Is it up to date?)
    java.lang.NoSuchMethodError: org/bukkit/configuration/file/...
    
    [15:24:04 ERROR]: Error occurred while enabling GGC v1.0.0 (Is it up to date?)
    java.lang.NullPointerException: null
    
而这些语句中的`NoClassDefFoundError`,`NoSuchMethodError`,`NullPointerException`,事实上揭示了这个报错的本质。我们可以根据这些错误，采取不同的解决方法。

### 插件类
1. `NoClassDefFoundException` / `ClassNotFoundException` / `NoSuchMethodError` / `NoSuchFieldError`  
原因： **内容缺失**  
解决方法： **尝试更换插件版本**,一般这个错误都是误用高版本或者低版本插件导致的,也可能因为该插件不兼容你所用的服务端，例如一些调用`NMS`的插件会对模组服务端不兼容。也有可能来自开发者的疏忽。  
  
2. `UnknownhostException` / `以Http开头的Exception`  
原因: **往往是插件连不上自己的更新服务器导致的。对于不想升级插件，服务器行为正常的服主没有太大影响**  
解决方法: 如果对于这个报错感到厌烦，您可以考虑使用[这个插件](https://www.mcbbs.net/thread-912870-1-1.html)进行**屏蔽**

3. `NullPointerException`  
原因: **十分广泛，一般无法由服主自己解决。可能来自于开发者的疏忽。**  
解决方法: 如果报错插件运行正常，您可以考虑使用[这个插件](https://www.mcbbs.net/thread-912870-1-1.html)对报错进行**屏蔽**，如果运行出错，你可以**参考下文**，向开发者**汇报**这个问题!  
 
**警告：如果点击箱子面板/箱子类插件时出现这个报错，就算没有明显问题，也要谨慎屏蔽！这有可能导致玩家刷取物品！！！**
   
4. 其他 ` Error/Exception`   
部分问题可以通过提供的报错信息得以解决，例如:
  

    IllegalStateException: Cannot make FireworkEffect without any color

这个方法和插件提示法异曲同工，不另行描述。  
 
## 模组类
   

1. 安装了客户端模组  
    

    [17:51:11] [main/WARN]: Error loading class:net/minecraft/client/Minecraft

任何和`client`有关的报错都有可能来自这个原因  
  
2. 重复安装了模组  


    DuplicateModsFoundException
3. 如果出现这种报错
  
  
    NullPointerException: Ticking block entity
请打开服务端 **config** 文件夹的 **forge.cfg** ,并将其中`B:removeErroringEntities`和`B:removeErroringTileEntities`两项改为**true**
  
  
如果以上方法无法让您解决问题，请向**开发者汇报**或在**问答板块求助**这个问题!不要拘束，这是双赢的。  

---

## 四.如何合理地提问和汇报错误？  

在汇报或提问的过程中，请**详细地描述报错情景**，提供**服务端类型和版本号**，提供**插件和模组列表**，并提供**没有精简过的完整报错**，甚至**报错的一部分上下文**（**日志文件**），或者**完整的崩溃日志**，大部分问题都可以得到解决。

## 扩展阅读

[[教程] 自己动手排查报错](https://www.mcbbs.net/thread-729487-1-1.html)，海螺螺著。

[怎么看崩溃报告和Timings？](https://www.mcbbs.net/thread-860103-1-1.html)，森林蝙蝠著。

