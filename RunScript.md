# 第一节 认识启动脚本

  Minecraft开服离不开启动脚本（不建议使用开服器）(作者使用jdk1.8.0_171 x64测试)

配置好的开服脚本:[点击跳转](#配置好的脚本)

```
bash
@ECHO OFF
java -Xmx1G -Xms1G -jar paper.jar
pause
```

这就是我们在[第一章第一节](First.md)所使用的启动脚本，我们会以这个为基础进行修改与优化。本部分着重讲解JVM优化

启动参数分为3类：

> - 标准参数（-）：所有的JVM实现都必须实现这些参数的功能，而且向后兼容；
> - 非标准参数（-X）：默认jvm实现这些参数的功能，但是并不保证所有jvm实现都满足，且不保证向后兼容；
> - 非Stable参数（-XX）：此类参数各个jvm实现会有所不同，将来可能会随时取消，需要慎重使用；

例：上面的"-jar"就属于标准参数

## 1. 标准参数
|参数|说明|
| :------------ | :------------ |
|-jar|执行jar文件|
|-client|设置jvm使用client模式，特点是启动速度比较快，但运行时性能和内存管理效率不高|
|-server|设置jvm使用server模式，特点是启动速度比较慢，但运行时性能和内存管理效率高，适用于生产环境。在64位的jdk环境下将默认启用该模式|

如果想了解更多的参数可以去翻阅官方文档：[官方文档](http://www.oracle.com/technetwork/java/javase/documentation/index.html "官方文档")

## 2. 内存设置
|参数|说明|
| :------------ | :------------ |
|-Xmx[size]|设置最大内存大小，默认为物理内存的1/4或者1G，需大于2M，可以指定单位k(K),m(M),g(G) 下同|
|-Xms[size]|设置初始化内存大小，默认为物理内存的1/64，需大于1M|

这样我们的启动参数就变成了(请根据自己的实际情况进行修改)

```bash
@ECHO OFF
java -server -Xmx8g -Xms4g -jar spigot-1.8.8-R0.1-SNAPSHOT-latest.jar
pause
```

如果开的是Mod服（Bukkit+Forge 或 Sponge）建议将`-Xms`与`-Xmx`的大小设置相同

## 3.行为参数

|参数|说明|
| :------------ | :------------ |
|-XX:+AggressiveOpts|加快编译|
|-XX:-DisableExplicitGC|禁止调用System.gc()，但jvm的gc仍然有效|
|-XX:+UseFastAccessorMethods|原始类型的快速优化|

<span id="jump"></span>
## 配置好的脚本
***注意：-Xmx8g -Xms8g 等请自行根据实际服务器配置进行设置***

***不要忘记改最后的“核心名.jar”！***
***不要忘记改最后的“核心名.jar”！***
***不要忘记改最后的“核心名.jar”！***

最后我们的启动参数就变成了这样：

------------

**纯净服**推荐脚本
```bash
@ECHO OFF
java -server -Xmx8g -Xms4g -XX:+AggressiveOpts -XX:+UseFastAccessorMethods -XX:-DisableExplicitGC -jar 核心名.jar
pause
```
根据我这个配置算出的CMSInitiatingOccupancyFraction值应小于或等于77所以这里我直接设置了70，大部分服务器可以直接设置70不需要改

**MOD服**建议使用下面的脚本，将`-Xms`与`-Xmx`的大小设置相同
```bash
@ECHO OFF
java -server -Xmx8g -Xms8g -XX:+AggressiveOpts -XX:+UseFastAccessorMethods -XX:-DisableExplicitGC -jar 核心名.jar
pause
```

如果你的jdk版本较高`java 8+`建议使用以下脚本
```bash
@ECHO OFF
java -server -Xms8g -Xmx8g -XX:+UseG1GC -XX:MaxGCPauseMillis=400 -XX:G1ReservePercent=15 -XX:ParallelGCThreads=4 -XX:ConcGCThreads=1 -XX:InitiatingHeapOccupancyPercent=40 -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -jar 核心名.jar
pause
```

#### 如何让你的服务器stop之后自动开服呢？

1. 只需要在脚本中在 `java -server ……` 的上一行加一个 `:开头的标签 `例如`:start`

2. 在java后加`cls`清屏（可加可不加） 

3. 最后`goto 刚才设置的标签`例如

最后脚本如下

```bash
@ECHO OFF
:start
java -server -Xmx8g -Xms4g -XX:+AggressiveOpts -XX:+UseFastAccessorMethods -jar 核心名.jar
cls
goto start
pause
```



本文部分内容引用自：[https://www.oracle.com/technetwork/java/hotspotfaq-138619.html](https://www.oracle.com/technetwork/java/hotspotfaq-138619.html "https://www.oracle.com/technetwork/java/hotspotfaq-138619.html")
