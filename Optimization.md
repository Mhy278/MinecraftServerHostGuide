# 第六节 配置文件优化

**译者注：本文翻译自 YouHaveTrouble 的 *[Minecraft server optimization guide](https://github.com/YouHaveTrouble/minecraft-optimization)* ，后文中以“~~删除线~~”标识的内容均为译者添加，不代表原文作者意见。**

针对游戏版本 1.16.5

根据这个[教程](https://www.spigotmc.org/threads/guide-server-optimization%E2%9A%A1.283181/)编写，其他引用内容会在引用处声明。

# 前言
没有任何一个教程提供的参数是完美的，每个服务器都应该从自身出发，根据实际情况不断微调，做出符合需求的设置。本教程旨在帮助您了解哪些参数会对性能产生影响，以及它们反映到游戏内的实际效果。 ~~（如果您认为本教程中的内容有误，可以在原文仓库发起PR）~~

# 准备

## 服务端核心
不同服务端核心可能会在性能和API接口方面存在巨大差异。 目前市面上常见的服务端核心有很多，以下是我的建议：

首选推荐:
* [Paper](https://github.com/PaperMC/Paper) - 非常流行的服务端核心，旨在提高性能并优化游戏内部机制。
* [Tuinity](https://github.com/Spottedleaf/Tuinity) - Paper 的分支，旨在更进一步提高服务端性能
* [Airplane](https://github.com/Technove/Airplane) - Tuinity 的分支，旨在再进一步提高服务端性能 ~~（禁止套娃）~~
* [Purpur](https://github.com/pl3xgaming/Purpur) - Airplane 的分支，旨在提高更多的可配置项 ~~（禁止禁止套娃）~~

傻孩子，快跑啊:
* Yatopia - "这个 Paper 的分支未曾设想地反向优化!" - [KennyTV点名批评](https://github.com/KennyTV/list-of-shame) ，我无Fuck说。
* 任何声称可以完美多线程的服务端核心 - 99.99% 都是画饼带师 ~~（Arclight赛高！）~~
* Bukkit/Craftbukkit/Spigot - 老到掉牙了
* 在运行时启用/禁用/重新加载插件的任何插件/软件。别问，问就是[看这儿](#plugins-enablingdisabling-other-plugins)

## 地图预生成
预生成地图是优化低性能服务器最重要步骤之一。这对面板服帮助巨大,因为它们无法完全异步加载区块。  你可以尝试像 **[chunky](https://github.com/pop4959/Chunky)** 插件来预生成世界。记得设定好一个世界边界，这样就不会再产生新的区块了。根据您设置的地图预生成大小不同，预生成需要的时间可能会很长。

切记！主世界、地狱和末地都需要单独设置世界边界。 原版设定下，地狱的大小比主世界小8倍，要是设错了边界，玩家可就要遁入世界边界之外了！

**一定要确认设置好原版世界边界 (`/worldborder set [半径]`), 因为它会限制某些特性，比如导致开藏宝图时的卡顿。**

# 配置文件

## 网络

### [server.properties]

#### network-compression-threshold

`从这儿开始调试比较好：256`

服务端每当一个数据包的大小达到设定的值就会压缩它。因此设定值越大CPU的压力就越小，但带宽占用也就越大了，将其设置为 -1 可禁用。设置的太高有可能会迫害到网速较慢的玩家。 如果您的服务器网络拓扑中存在延迟较低的局域网，将其禁用即可。

### [purpur.yml]

#### use-alternate-keepalive

`一般建议：true`

启用 Purpur 的心跳检测可以让网络状况较差的玩家不会总是掉线，已知与 TCPShield 冲突。

> 启用此功能后，每秒会向玩家发送一个心跳包，并且只有在 30 秒没有响应的情况下才会计为超时。
即使不按发包顺序回复也不会影响连接。妈妈再也不用担心玩家因为丢一个包就掉线啦！
~ https://pl3xgaming.github.io/PurpurDocs/Configuration/#use-alternate-keepalive

---
## 区块

### [spigot.yml]

#### view-distance

`从这儿开始调试比较好：4`

此项限制了玩家可见区块的渲染距离，也包括熔炉和植物生长，实质上就是游戏机制发生的最大距离。 应该在 **[spigot.yml]** 里设置， 它会覆盖 **[server.properties]** 里的设置，还可以分世界设置。但因为存在`no-tick-view-distance`（假视距），您可以将该参数设置为`3` 或 `4` 之类较低的值。假视距可以让玩家看到更远距离的同时降低负载。

### [paper.yml]

#### no-tick-view-distance

`从这儿开始调试比较好：7`

此项限制了玩家可看到的最大距离（区块）。 这可以在降低 `view-distance`的同时仍能让玩家看到更远的距离。切记！虽然超出`view-distance`的部分不会处理游戏机制，但还是要读地图，所以别滥用。 `10` 基本上是正常情况下的最大值。 目前算法下，无论其视距设置多少，都要将区块发送到客户端，因此设置较高的值可能会迫害网速较慢的玩家。

#### delay-chunk-unloads-by

`从这儿开始调试比较好：10`

此项限制玩家离开后该区块多久后才会卸载，可减轻玩家在一些较热门区块间反复横跳导致不断加载卸载产生的负载，但设定值太高可能会让过多的区块同时加载。如果一些区块过于热门，可以考虑将其设置为永久区块。

#### max-auto-save-chunks-per-tick

`从这儿开始调试比较好：8`

通过降低世界增量保存的速度来提高平均性能。如果有20-30名玩家在线，您应该试着将该项设置为`8` 以保证服务器的安全运行。 如果增量保存无法及时完成，则 Bukkit 将立即自动保存剩余的区块并再次开始该过程。

#### prevent-moving-into-unloaded-chunks

`一般建议：true`

防止玩家进入未加载的区块，以避免在主线程同步加载区块时拖长其造成的卡顿。假视距越小，玩家进入未加载区块的可能性就越大。

#### entity-per-chunk-save-limit

```yaml
从这儿开始调试比较好:

    arrow: 16
    dragonfireball: 3
    egg: 8
    ender_pearl: 8
    fireball: 8
    firework: 8
    largefireball: 8
    lingeringpotion: 8
    llamaspit: 3
    shulkerbullet: 8
    sizedfireball: 8
    snowball: 8
    spectralarrow: 16
    splashpotion: 3
    thrownexpbottle: 3
    trident: 16
    witherskull: 4
```

此项可以限制每个区块的指定类型实体可保存的数量。

您应该为每一种实体设定合适的值来降低在服务器加载时的负载。一般情况下都设置为`10`也未尝不可，但还是建议您手动调试到合适的值以防止过多的刷怪塔之类的卡服。


#### armor-stands-tick

`一般建议：false`

此项可以防止盔甲架被水推走或受到重力的影响。

在大多数情况下，您可以安全地将其设置为`false`。 但如果您使用盔甲架或任何修改其行为的插件遇到了问题，就设置为`true`吧。 

#### armor-stands-do-collision-entity-lookups

`一般建议：false`

禁用盔甲架碰撞。

---

## 生物

### [bukkit.yml]

#### spawn-limits

```yaml
从这儿开始调试比较好:

    monsters: 20
    animals: 5
    water-animals: 2
    water-ambient: 2
    ambient: 0
```

限制生物生成最大数量的机制是 `当前玩家数 * 限量`。`per-player-mob-spawn` 是针对每名玩家周边的生物限制，以确保生物在各玩家间平均生成， 这么做有利有弊；虽然这么做是会降低服务器负载，但也会降低游戏趣味。

如果您设置的`mob-spawn-range`比较合理，那这一项就可以降低到`20`以下，因为把` mob-spawn-range`调低可以让每个玩家周围生成的怪物更靠近玩家。

如果您使用的是 Tuinity 核心，您还可以在 **[tuinity.yml]** 里单独设置每个世界的限制。

#### ticks-per

```yaml
从这儿开始调试比较好:

    monster-spawn: 10
    animal-spawns: 400
    water-spawns: 400
    water-ambient-ambient: 400
    ambient-spawns: 400
```

此项限制服务器生成某些活动实体的频率（以tick为单位）。

水中或周围的生物并不会很快去世，所以并不需要每tick都生成。对怪物来说，就算是在刷怪塔里，略微增加生成时间也不会影响生成率。一般情况下，此项所有值都应该大于`1`。调高此值可以更好的控制怪物禁止生成的区域。

### [spigot.yml]

#### mob-spawn-range

`从这儿开始调试比较好：2`

此项可以限制玩家周围生成怪物的范围（以区块为单位）。

根据服务器的类型和在线玩家人数不同，您可能会想降低此值以及 **[bukkit.yml]** 的` spawn-limits`。降低此值会让玩家感到周围好像有更多生物，所以该值应该小于等于您设置的视距，且永远不大于怪物强制清理范围。

#### entity-activation-range

```yaml
从这儿开始调试比较好:

      animals: 16
      monsters: 24
      raiders: 48
      misc: 8
      water: 8
      villagers: 16
      flying-monsters: 48
```

此项限制玩家激活实体游戏机制的范围。

降低此值可提高性能，但会让远处的实体像个憨批。降得太低还可能会影响刷怪塔，尤其是刷铁塔。

#### entity-tracking-range

```
从这儿开始调试比较好:

      players: 48
      animals: 48
      monsters: 48
      misc: 32
      other: 64
```

此项限制玩家可以看见实体的距离（以方块为单位）。

实体本身生成后一直存在，只是在玩家进入可见范围之前不会被发送给客户端。所以如果此项设置的过低，怪物怕就是要和玩家撞脸了。一般来说此值都应该大于 `entity-activation-range`。

#### tick-inactive-villagers

`一般建议：false`

此项可以管理在激活实体游戏机制范围外的村民是否保留游戏机制。

禁用该项可以提高性能，但也有较大可能影响游戏趣味，比如刷铁塔可能会无效。

#### nerf-spawner-mobs

`译者建议：false`

此项可以让刷怪笼生成的怪物没有AI机制，什么也不会做。~~（会降低游戏趣味）~~ 

还可以在 **[paper.yml]** 中将` spawner-nerfed-mobs-should-jump`更改为`true`让它们可以在水中跳跃。

### [paper.yml]

#### despawn-ranges

```yaml
从这儿开始调试比较好:

      soft: 30
      hard: 56
```

此项可以限制实体消失的范围（以方块为单位）。降低此值可以更快清理原理玩家的生物， 您应该将`soft`保持在`30`附近，且将`hard`调到比实际的可视距离大一点，这样当玩家离开加载过的区块时，怪物并不会立即消失（因为有 **[paper.yml]** 中的`delay-chunk-unloads-by`存在，效果会很棒）。

当怪物离开了`hard`范围，它才会立即消失。在`soft`和`hard`之间会随机消失，所以`hard`应该大于`soft`。您应该按照`(view-distance * 16) + 8`的公式来进行此项的调试。这能包含到玩家离开之后还没来得及卸载的区块。

#### per-player-mob-spawns

`一般建议：true`

此项可以决定生物生成是否围绕每个玩家进行。这样可以避免某些玩家建造刷怪塔霸占了大量的怪物，还普通玩家一个更像原版单人生存的游戏体验，且可以调低`spawn-limits`。启用此项确实会吃一些性能，但只要设置好`spawn-limits`就不足为虑了。

#### max-entity-collisions

`从这儿开始调试比较好：2`

此项会覆盖 **[spigot.yml]** 中的`max-entity-collisions`。 此项可以限制一个实体一次可以计算多少碰撞。设为`0`将无法推动包括玩家在内的其他实体。 一般情况下，设成`2`就彳亍。

**注意：** 如果原版游戏规则中的`maxEntityCramming`值（小空间内最大实体数目）超过此项设定值，将会令这条原版游戏规则无效。

#### update-pathfinding-on-block-update

`一般建议：false`

禁用此项将减少寻路机制完成的次数，从而提高性能。 某些情况下，这将导致怪物反应更加迟钝， 因为他们只会被动地每5个tick（0.25秒）更新其路径。

#### fix-climbing-bypassing-cramming-rule

`一般建议：true`

启用此项将修复实体在攀爬过程中不受空间范围内最大实体数量限制的问题。

就算他们是在爪巴，也不能全挤到一个屋！

### [airplane.air]

#### max-tick-freq

`从这儿开始调试比较好：20`

此项限制了距离玩家最远的实体卸载游戏机制的最低数量。调高此值可能会提高性能，但也可能会影响刷怪塔或降低怪物智商。

#### activation-dist-mod

`从这儿开始调试比较好：9`

基于DEAR控制怪物清理的梯度。降低此值将减小DEAR，从而提高性能，但会影响实体间的交互，甚至会影响刷怪塔的运作。

**译者注：** Airplane 拥有一个独家的算法：动态实体激活范围（Dynamic Entity Activation Range，DEAR），详见[博文](https://blog.airplane.gg/dear-configuration/)。

### [purpur.yml]

#### dont-send-useless-entity-packets

`一般建议：true`

启用此项可禁止服务器发送空的位置更新数据包来节省带宽（默认情况下，即使实体未移动，服务器也会为每个实体发送此数据包）。但可能会导致利用客户端侧实体的插件出现一些问题。

#### aggressive-towards-villager-when-lagging

`一般建议：false`

如果服务端低于 **[purpur.yml]** 中`lagging-threshold` 设置的TPS阈值，启用此项会让僵尸不攻击村民。

#### entities-can-use-portals

`一般建议：false`

此项可以让只有玩家才可以使用传送门。 这可以防止实体在主线程上加载不必要的区块。

#### villager.brain-ticks

`从这儿开始调试比较好：2`

此项可以设置村民的更新兴趣点的频率（以tick为单位）。已知高于`3`会产生问题。

**译者注：** 兴趣点是村民的一个NBT，详见[百科](https://minecraft.fandom.com/wiki/Villager#Entity_data)中 Tags common to all mobs -> Brain -> memories 。

#### villager.lobotomize

`一般建议：true`

离玩家较远的村民被卸载了AI后只会按时补货。启用此项会让这些村民禁用自动寻路，重新激活AI时会恢复。


---

## 杂项

### [spigot.yml]

#### merge-radius

```yaml
从这儿开始调试比较好:

      item: 3.5
      exp: 4.0
```

此项限制了同类物品及经验球合并计算的距离，可减少地面未拾取物数量。

设置得太高会导致合并时像瞬间消失，也会使得物品穿透方块，可能破坏刷怪塔和刷经验塔。

经验球仅在生成时会合并。

#### hopper-transfer

`从这儿开始调试比较好：8`

漏斗移动一个物品的等待时间（以tick为单位）。
增加这个值会让拥有N多漏斗的服务器流畅一点，但是这会破坏掉基于漏斗的时钟，当设置的值太高时也有可能影响物品分类系统。

#### hopper-check

`从这儿开始调试比较好：8`

漏斗检查上方的物品或箱子的频率（以tick为单位）。 增加这个值会让拥有N多漏斗的服务器流畅一点，但是这会破坏掉基于漏斗的时钟，当设置的值太高时也有可能影响基于水流的物品分类系统。

### [paper.yml]

#### alt-item-despawn-rate

```yaml
从这儿开始调试比较好：

      enabled: true
      items:
          COBBLESTONE: 300
```

此项可以设置异步时间（tick），以比原版更快或更慢的速度刷新某些类型的已清除物品。可以使用该项替代扫地姬及`merge-radius`来提高性能。

#### use-faster-eigencraft-redstone

`一般建议：true`

此项启用后，红石机制将切换为更快的异步版本，该版本减少了不必要的方块更新，从而降低负载。 启用此功能可以显着提高性能，且不会影响原版游戏机制，甚至可以解决来自 CraftBukkit 的一些红石机制问题。

#### disable-move-event

`一般建议：false`

当且仅当有插件正在监听`InventoryMoveItemEvent` 事件时才会触发。 如果您并不在乎是否有插件在监听这个事件，就可以设为`true`。

**如果您想使用监听此事件的插件，请不要设为`true`，如保护插件！**

#### mob-spawner-tick-rate

`从这儿开始调试比较好：2`

刷怪笼进行一次判定的频率（以tick为单位）。

当存在许多刷怪笼时，更高的值意味着更少的延迟但当你设置的太高时（相对于你的刷怪笼延迟）怪物生成率将会被减少。

#### optimize-explosions

`一般建议：true`

此项设为`true`会优化原版爆炸算法，但爆炸伤害计算会略有不准，不过问题不大。

#### enable-treasure-maps

`一般建议：false`

当你与村民交易探险家地图时会生成探险家地图，如果尝试定位的结构不在您的预生成地图范围内，则可能会拖垮服务器线程。 当您预生成世界并设置了原版世界边界时，启用此功能才是安全的。

#### treasure-maps-return-already-discovered

`一般建议：true`

此项的默认值会强制新生成的藏宝图寻找未探索的结构，这些结构通常在您预生成的地图之外。

将此属性设置为`true`可以使藏宝图寻找已经发现的结构，否则在生成新的藏宝图时，服务器可能会卡顿或崩溃。

#### grass-spread-tick-rate

`从这儿开始调试比较好：4`

泥土方块上草皮或菌丝传播所需的时间（以tick为单位）。

设置的太小会使大范围的草皮或菌丝生成需要更长时间，如果您想降低它又不想影响扩散率，设为`4`左右就彳亍。

#### container-update-tick-rate

`从这儿开始调试比较好：1`

两个容器间物品更新的时间（以tick为单位）。

如果您遇到了容器物品更新问题，调大此值可能有所帮助，但也可能会使玩家在与容器内物品交互时出错（出现幽灵物品）。

#### non-player-arrow-despawn-rate

`从这儿开始调试比较好：20`

怪物射出的箭击中某物后消失的时间（以tick为单位）。
因为玩家无法捡起它们，将其设置为`20`（1秒）之类的值就彳亍。

#### creative-arrow-despawn-rate

`从这儿开始调试比较好：20`

创造模式玩家射出的箭击中某物后消失的时间（以tick为单位）。
因为玩家无法捡起它们，将其设置为`20`（1秒）之类的值就彳亍。

### [purpur.yml]

#### disable-treasure-searching

`译者建议：false`

此项可阻止海豚执行寻找结构的[行为](https://minecraft.fandom.com/zh/wiki/%E6%B5%B7%E8%B1%9A#.E8.A1.8C.E4.B8.BA) ~~（会降低游戏趣味）~~

#### teleport-if-outside-border

`一般建议：true`

如果玩家误入世界边界之外，则可以将其传送到该世界的复活点。 这个功能比较好的一点在于，可以无视原版世界的边界，减轻虚空对玩家的伤害。

---

## 辅助

### [paper.yml]

#### anti-xray

`一般建议：true`

反矿物透视，详细配置可见[Stonar96的推荐参数](https://gist.github.com/stonar96/ba18568bd91e5afd590e8038d14e245e)。此功能会吃一些性能，但它比任何单独的反矿透插件都好使。一般也不用计较性能影响，可以忽略不计。

#### remove-corrupt-tile-entities

`一般建议：true`

如果您的控制台有与实体有关的报错，请将其更改为`true`。 这将直接删除报错的实体，而不是忽略报错。 如果是一直在刷与实体有关的报错还是趁早看看吧，这个功能治标不治本。

#### nether-ceiling-void-damage-height

`一般建议：127`

如果此值大于`0`，在此高度之上的玩家就会像坠入虚空一样不断受到伤害，一般可以用来防止玩家在地狱天花板上违章建筑。原版的地狱高度是128，所以设成`127`就可以了。因此如果您修改了地狱世界的高度，那就把它设成比地狱世界最大高度小1格。

---

# Java 启动参数
[Paper 及其分支对1.17开始的游戏版本将需要Java11或更高的版本](https://papermc.io/forums/t/java-11-mc-1-17-and-paper/5615)。喜报！2021年的您终于要更新服务器上的 Java 版本啦！ Oracle 版的 Java 已经不配我们去下载了，目前推荐 [Amazon Corretto](https://aws.amazon.com/corretto/) 和 [AdoptOpenJDK](https://adoptopenjdk.net)。其他JVM实现（如OpenJ9）也可以用，但是Paper并不支持，且已知存在兼容问题，不建议使用。

您可以通过配置您的垃圾回收器(GC)来减少因大型垃圾回收任务导致的卡顿，
可以在[这里](https://mcflags.emc.gs/)看到建议的启动参数。

注意！此建议不适用于其他的JVM实现。

# "自欺欺人"的负优化插件

## 扫地姬
很鸡肋的插件，完全可以通过`merge-radius`和`alt-item-despawn-rate`来替代。
而且有一说一，这些插件的可配置性一般较差，甚至扫地占用的性能比不扫地还大。

## 生物堆叠插件
很难证明这玩意有用。堆叠自然生成的实体只会让服务器更卡，因为这会让服务器生成更多的怪物。我觉得只有在那些拥有大量刷怪笼的服务器上对刷怪笼生成的怪物用这个才是“可以接受”的使用场景。

## 加载/卸载其他插件的管理插件
在运行时任何加载或卸载插件的操作都是非常危险的。 加载类似的插件会导致跟踪数据的致命错误，而卸载插件可能会因失去依赖而产生错误。 这个 `/reload` 命令同样也存在这些问题，详见[me4502的博文](https://madelinemiller.dev/blog/problem-with-reload/)。

# 什么是卡顿？ - 分析负载占用

## MSPT
Paper 提供了一个 `/mspt` 命令可以获取服务器处理最近的Tick所花费的时间。

如果您看到的第一个和第二个值小于50，那么恭喜： 您的服务器整挺好！

如果第三个值超过50，则意味着至少有1个Tick花费了较长的时间。

但这是正常的波动，时有发生不足为怪。

（译者注：MSPT与TPS的区别可查看[百科](https://minecraft.fandom.com/zh/wiki/%E5%88%BB#.E6.B8.B8.E6.88.8F.E5.88.BB)）

## Timings
Timings 是查看服务器卡顿时在做咩的好方法。它可让您确切地查看花费了最长时间的任务到底是哪个闸总。

要获取服务器的Timings，您只需要执行`/ timings paste`命令，然后查看提供的链接即可。

这也是最基本的故障排除工具，您最好带上它去问大佬“我的服务器为什么这么卡？”

## Spark
[Spark](https://github.com/lucko/spark) 是一个可分析负载占用的插件。您可阅读[百科](https://github.com/lucko/spark/wiki/Commands)来学习使用它。[这里](https://github.com/lucko/spark/wiki/Finding-the-cause-of-lag-spikes)还有个教你怎么找到卡顿罪魁祸首的教程。


# 常见误区与好的习惯

本段旨在介绍新手开服时的常见误区。

## 经常备份
要问我对于备份的态度，那只有两种：正在备份和开始备份的路上。

数据丢失是每个人必然会遇到的！条件允许的情况下尽量多的备份、采取多种备份手段保证数据安全才是重中之重！

## 别用旧版本软件！
运行旧版本的软件，就是将已知的漏洞暴漏给外界！玩家也必须得拿旧版本客户端连接您旧版本的服务器，尽管可以魔改协议来兼容，但还是不如原配的好。

## 真的再别用 Bukkit 和 Spigot 了！
它们虽然仍然在维护，但做的仅仅只是跟进官方的特性更新，对于性能和稳定性毫无优化！

请尝试上文中我推荐的优秀服务端核心吧，它们都兼容 Bukkit 和 Spigot 的插件，甚至可以运行的更好。

## 真的再别用面板服了！
我完全理解面板服是目前价格最便宜的开服方案，但它各方面也确实是最差的。

面板服共享CPU、硬盘和带宽等等资源，您无法享受到这些硬件的全部性能，而且绝大多数面板服都会超售超开，出售的资源比实际的要多得多，这对于所有服务器来说都是极其致命的！

## 避免用数据包执行命令
使用数据包执行命令是非常落后的行为，虽然开始可能只有很少的玩家使用，但随着服务器规模的扩大，这种行为很快会落后于服务器的发展。但修改生物群落、战利品表（怪物、方块的掉落）等的数据包性能雀食很好，不过您最好还是另谋插件吧。


[`SOG`]: https://www.spigotmc.org/threads/guide-server-optimization%E2%9A%A1.283181/
[server.properties]: https://minecraft.gamepedia.com/Server.properties
[bukkit.yml]: https://bukkit.gamepedia.com/Bukkit.yml
[spigot.yml]: https://www.spigotmc.org/wiki/spigot-configuration/
[paper.yml]:  https://paper.readthedocs.io/en/latest/server/configuration.html
[purpur.yml]: https://purpur.pl3x.net/docs
[tuinity.yml]: https://github.com/Spottedleaf/Tuinity/wiki/Config
[airplane.air]: https://airplane.gg/config
