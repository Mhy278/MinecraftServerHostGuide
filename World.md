# 第五节 什么是世界？
服务端的世界文件都存储在了根目录的对应文件夹下。

Minecraft 原版目前有三个世界，分别是主世界`world`，地狱`nether`，末地`the end`。这些世界的文件对应的保存在了服务端根目录的`world`、`world_nether`与`world_the_end`文件夹中。  
每个世界的方块都按照X和Z每16*16为一个单位，被分成了一个一个的区块。服务器的世界是按照区块为单位储存并读取使用的。

在这里以主世界为例，说明世界是怎样保存的。
打开服务端根目录的`world`文件夹：
1. `advancements`文件夹: 储存着玩家的成就数据，每个玩家对应一个文件，老版本中文件名是玩家名，新版本按照UUID储存。  
2. `playerdata`文件夹: 储存着玩家的数据信息，例如血量、经验值等数据。  
3. `poi`文件夹: 存储世界内与村民有关的数据。  
4. `region`文件夹: 储存区块信息。  
5. `stats`文件夹: 储存玩家在此世界游玩时的统计信息。  
6. `level.dat`文件: 储存该世界的全局的主干信息。  
7. `data`文件夹: 其中的`villages.dat`文件储存的是村民的数据信息, `idcounts.dat`和`map_<XXX>.dat`储存的是玩家合成的地图有关的数据.  

其他世界的文件夹大同小异。

## 如何使用多世界插件
在这里我们以常见的 Multiverse-Core 插件为例：

### 指令
这里简单的列举几个常见指令：

命令 | 说明
--- | ---
/mv list | 列出服务器内所有的世界
/mv reload | 重新加载配置文件
/mv info [世界名] [页码] | 列出这个世界的所有信息
/mv create [世界名] [环境类型] | 创建世界
/mv clone [复制出的世界名] [被复制的世界名] | 克隆一个世界
/mv import [世界名] [环境类型] | 导入一个世界
/mv unload [世界名] | 卸载某一世界 *
/mv load [世界名] | 加载某一世界 *
/mv remove [世界名] | 移除某一世界 *
/mv delete [世界名] | 彻底删除某一世界
/mv regen [世界名] | 重新生成某一世界(清空并重新生成)
/mv setspawn | 设置出生点
/mv tp [玩家名] [世界名] | 将该玩家传送至某一世界
/mv who [世界名] | 查看某一世界都有哪些玩家

**注: 卸载指的是不删除该世界的数据文件，仅仅把该世界从内存上移除。**  
**注2: 如果加载的是服务器原来没有的世界，加载某一世界前需要导入该世界（import）。**  
**注3: 移除指的是世界的数据文件保留，但是在多世界插件的配置信息中将会删除该世界。被移除的世界必须通过导入才能重新被服务器加载。**


### 配置
`config.yml` MultiverseCore 插件全局配置
```yaml
multiverse-configuration:
        ==: com.onarandombox.MultiverseCore.MultiverseCoreConfiguration
        enforceaccess: true
        # 显示聊天世界前缀
        prefixchat: true
        # 设置聊天世界前缀 %world%代表世界名, %chat%代表玩家聊天内容
        prefixchatformat: '[%world%]%chat%'
        # 是否开启异步聊天(不建议修改)
        useasyncchat: true
        teleportintercept: true
        # 是否锁定出生世界
        firstspawnoverride: true
        displaypermerrors: true
        globaldebug: 0
        silentstart: true
        # 消息冷却时间
        messagecooldown: 5000
        # 这！个！不！要！改！
        version: 2.6
        # 默认的玩家出生世界
        firstspawnworld: world
        # 玩家传送的冷却时间（毫秒）
        teleportcooldown: 1000
        defaultportalsearch: false
        portalsearchradius: 128
```

`worlds.yml` 单一世界配置 
```yaml
    world:
        ==: MVWorld
        hidden: false
        # 该世界的前缀
        alias: ''
        # 前缀的颜色 (可以设置的值: AQUA, BLACK, BLUE, DARKAQUA, DARKBLUE, DARKGRAY, DARKGREEN,DARKPURPLE, DARKRED, GOLD, GRAY, GREEN, LIGHTPURPLE, RED, YELLOW, WHITE)
        color: WHITE
        # 样式 (可以设置的值: NORMAL 正常, BOLD 加粗, STRIKETHROUGH 删除线, UNDERLINE 下划线 , ITALIC 斜体)
        style: NORMAL
        # 是否允许PVP
        pvp: false
        scale: 1.0
        # 在该世界死亡后指定复活世界(会复活在这个世界的出生点, 留空代表在本世界出生点复活)
        respawnWorld: ''
        # 是否允许天气变化
        allowWeather: true
        # 地图难度 (可以设置的值: PEACEFUL 和平, EASY 简单, NORMAL 中等, HARD 难)
        difficulty: PEACEFUL
        spawning:
            ==: MVSpawnSettings
        animals:
            ==: MVSpawnSubSettings
            # 是否允许生成动物
            spawn: true
            # 生成动物的速率
            spawnrate: -1
            exceptions: []
            #以下同理, 是设置怪物
        monsters:
            ==: MVSpawnSubSettings
            spawn: true
            spawnrate: -1
            exceptions: []
        entryfee:
            ==: MVEntryFee
            # 进入该世界需要支付的金额
            amount: 0.0
            # 货币种类(就是物品ID, -1代表从经济插件扣钱)
            currency: -1
        hunger: true
        autoHeal: true
        adjustSpawn: true
        portalForm: ALL
        # 玩家默认游戏模式
        gameMode: SURVIVAL
        keepSpawnInMemory: false
        spawnLocation:
            ==: MVSpawnLocation
            x: -221.0
            y: 63.0
            z: 219.0
            yaw: 0.0
        autoLoad: true
        bedRespawn: false
        worldBlacklist: []
        # 世界类型(可以设置的值: NETHER, END, NORMAL)
        environment: NORMAL
        # 种子
        seed: '4337274906620320171'
        generator: 'null'
        playerLimit: -1
        # 是否默认允许飞行
        allowFlight: false
```

### 权限
具体内容请见 权限 章。

这里简单的列举几个常见权限：

权限 | 解释
--- | ---
`multiverse.help` | 输入/mv获得帮助信息
`multiverse.access.name` | 玩家进入世界
`multiverse.exempt.name` | 玩家免费进入世界
`mv.bypass.gamemode.name` | 玩家的游戏模式不改变地进入世界
`multiverse.teleport.self.name` | 玩家传送自己到另一个世界
`multiverse.teleport.other.name` | 玩家带着其他玩家一起传送到另一个世界
`multiverse.core.create` | 玩家创建世界
`multiverse.core.import` | 玩家导入世界
`multiverse.core.modify` | 玩家修改世界
`multiverse.core.modify.add` | 玩家使用修改世界中添加功能
`multiverse.core.modify.clear` | 玩家使用修改世界中清除功能
`multiverse.core.modify.modify` | 玩家使用修改世界中修改功能
`multiverse.core.modify.remove` | 玩家使用修改世界中删除功能
`multiverse.core.modify.set` | 玩家使用修改世界中设置功能
`multiverse.core.reload` | 重新加载配置文件指令
`multiverse.core.delete` | 删除世界
`multiverse.core.unload` | 卸载世界

### 使用 Multiverse-Core 在服务器里导入地图

先把地图文件夹放在服务器的根目录下，在后台输入  
```
/mv import [地图名(跟文件夹名相同)] normal
```
地图即导入完毕. OP可以输入指令进入该世界:  
```
/mv tp [地图名]
```


