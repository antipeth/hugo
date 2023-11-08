---
title: "docker搭建泰拉瑞亚Tshock服务器"
date: 2023-11-05T00:48:58+08:00
draft: false
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
url: "/game/0-0-1"
keywords:
- 游记
categories:
- 游
tags:
- 游记
---

# 环境

Debian11

# 安装docker

```bash
apt update
apt install docker
```

# 部署镜像

## 拉取镜像

```bash
docker pull ryshe/terraria
```

## 运行容器

### 创建一个世界

```bash
docker run -it -p 7777:7777 --rm -v $HOME/terraria/world:/root/.local/share/Terraria/Worlds ryshe/terraria:latest
```

这里的 `$HOME` 指的是你当前所在目录。也可以指定一个想要存放游戏文件的目录。

比如我的目录是/var/lib/docker/volumes/terraria/_data

所以我的命令是

```bash
docker run -it -p 7777:7777 --rm -v /var/lib/docker/volumes/terraria/_data/terraria/world:/root/.local/share/Terraria/Worlds ryshe/terraria:latest
```

执行后会出现服务器的界面

![](https://img.0pt.im/game/built-tr/1-1.png)

接下来根据提示，创建一个世界。

![](https://img.0pt.im/game/built-tr/1-2.png)

创建完了。

这个时候先可以退出ssh。

### 配置Tshock强制开荒

在刚才的`$HOME/terraria/world`目录下，找到配置文件`config.json`和`sscconfig.json`

![](https://img.0pt.im/game/built-tr/1-3.png)

这是tshock的配置文件，可以参考下面我的配置文件进行更改。

```config.json
{
  "Settings": {
    "ServerPassword": "",  // 服务器密码
    "ServerPort": 7777,  // 服务器端口
    "MaxSlots": 8,  // 最大玩家槽位
    "ReservedSlots": 20,  // 预留槽位
    "ServerName": "",  // 服务器名称
    "UseServerName": false,  // 使用服务器名称
    "LogPath": "tshock/logs",  // 日志路径
    "DebugLogs": false,  // 调试日志
    "DisableLoginBeforeJoin": false,  // 加入前禁止登录
    "IgnoreChestStacksOnLoad": false,  // 加载时忽略储物箱叠放
    "WorldTileProvider": "default", 
    "AutoSave": true,  // 自动保存
    "AnnounceSave": true,  // 保存时公告
    "ShowBackupAutosaveMessages": true,  // 显示备份和自动保存信息
    "BackupInterval": 10,  // 备份间隔
    "BackupKeepFor": 240,  // 保留备份时间
    "SaveWorldOnCrash": true,  // 崩溃时保存世界
    "SaveWorldOnLastPlayerExit": true,  // 最后一个玩家退出时保存世界
    "InvasionMultiplier": 1,  // 入侵倍增器
    "DefaultMaximumSpawns": 5,  // 默认最大生成数量
    "DefaultSpawnRate": 600,  // 默认生成速率
    "InfiniteInvasion": false,  // 无限入侵
    "PvPMode": "normal",  // PvP模式
    "SpawnProtection": false,  // 出生点保护
    "SpawnProtectionRadius": 10,  // 出生点保护半径
    "RangeChecks": true,  // 范围检查
    "HardcoreOnly": false,  // 仅极限模式
    "MediumcoreOnly": false,  // 仅中核模式
    "SoftcoreOnly": false,  // 仅软核模式
    "DisableBuild": false,  // 禁止建筑
    "DisableHardmode": false,  // 禁止极限模式
    "DisableDungeonGuardian": false,  // 禁止地牢守卫者
    "DisableClownBombs": false,  // 禁止小丑炸弹
    "DisableSnowBalls": false,  // 禁止雪球
    "DisableTombstones": false,  // 禁止墓碑
    "DisablePrimeBombs": false,  // 禁止机械首领炸弹
    "ForceTime": "normal",  // 强制时间
    "DisableInvisPvP": false,  // 禁止隐形PvP
    "MaxRangeForDisabled": 10,  // 禁用的最大范围
    "RegionProtectChests": false,  // 区域保护储物箱
    "RegionProtectGemLocks": false,  // 区域保护宝石锁
    "IgnoreProjUpdate": false,  // 忽略投射物更新
    "IgnoreProjKill": false,  // 忽略投射物击杀
    "AllowCutTilesAndBreakables": false,  
    "AllowIce": false,  // 允许冰
    "AllowCrimsonCreep": true,  // 允许猩红蔓延
    "AllowCorruptionCreep": true,  // 允许腐化蔓延
    "AllowHallowCreep": true,  // 允许神圣蔓延
    "StatueSpawn200": 3, 
    "StatueSpawn600": 6,  
    "StatueSpawnWorld": 10,  
    "PreventBannedItemSpawn": true,  // 防止被封禁物品生成
    "PreventDeadModification": true,  // 防止死亡修改
    "PreventInvalidPlaceStyle": true,  // 防止无效的放置样式
    "ForceXmas": false,  // 强制圣诞节
    "ForceHalloween": false,  // 强制万圣节
    "AllowAllowedGroupsToSpawnBannedItems": false,  // 允许有权限的组生成被封禁物品
    "RespawnSeconds": 15,  // 重生时间（秒）
    "RespawnBossSeconds": 10,  // Boss战重生时间（秒）
    "AnonymousBossInvasions": true,  // 匿名Boss入侵
    "MaxHP": 500,  // 最大生命值
    "MaxMP": 200,  // 最大魔力值
    "BombExplosionRadius": 5,  // 炸弹爆炸半径
    "GiveItemsDirectly": false,  // 直接给予物品
    "DefaultRegistrationGroupName": "default",  // 默认注册组名称
    "DefaultGuestGroupName": "guest",  // 默认访客组名称
    "RememberLeavePos": true,  // 记住离开位置
    "MaximumLoginAttempts": 10,  // 最大登录尝试次数
    "KickOnMediumcoreDeath": false,  // 中核模式死亡踢出
    "MediumcoreKickReason": "死亡导致踢出",  // 中核模式踢出原因
    "BanOnMediumcoreDeath": false,  // 中核模式死亡封禁
    "MediumcoreBanReason": "死亡导致封禁",  // 中核模式封禁原因
    "DisableDefaultIPBan": false,  // 禁用默认IP封禁
    "EnableWhitelist": false,  // 启用白名单
    "WhitelistKickReason": "您不在白名单中。",  // 白名单踢出原因
    "ServerFullReason": "服务器已满",  // 服务器已满原因
    "ServerFullNoReservedReason": "服务器已满。没有预留槽位。",  // 服务器已满没有预留槽位原因
    "KickOnHardcoreDeath": false,  // 极限模式死亡踢出
    "HardcoreKickReason": "死亡导致踢出",  // 极限模式踢出原因
    "BanOnHardcoreDeath": false,  // 极限模式死亡封禁
    "HardcoreBanReason": "死亡导致封禁",  // 极限模式封禁原因
    "KickProxyUsers": true,  // 踢出代理用户
    "RequireLogin": true,  // 需要登录
    "AllowLoginAnyUsername": true,  // 允许任何用户名登录
    "AllowRegisterAnyUsername": true,  // 允许注册任何用户名
    "MinimumPasswordLength": 4,  // 最小密码长度
    "BCryptWorkFactor": 7,  // BCrypt工作因子
    "DisableUUIDLogin": false,  // 禁用UUID登录
    "KickEmptyUUID": false,  // 踢出空UUID
    "TilePaintThreshold": 15,  // 瓷砖绘制阈值
    "KickOnTilePaintThresholdBroken": false,  // 瓷砖绘制阈值破坏踢出
    "MaxDamage": 51175,  // 最大伤害
    "MaxProjDamage": 51175,  // 最大投射物伤害
    "KickOnDamageThresholdBroken": false,  // 伤害阈值破坏踢出
    "TileKillThreshold": 1060,  // 瓷砖击杀阈值
    "KickOnTileKillThresholdBroken": false,  // 瓷砖击杀阈值破坏踢出
    "TilePlaceThreshold": 1032,  // 瓷砖放置阈值
    "KickOnTilePlaceThresholdBroken": false,  // 瓷砖放置阈值破坏踢出
    "TileLiquidThreshold": 1050,  // 瓷砖液体阈值
    "KickOnTileLiquidThresholdBroken": false,  // 瓷砖液体阈值破坏踢出
    "ProjIgnoreShrapnel": true,  // 投射物忽略破片
    "ProjectileThreshold": 1050,  // 投射物阈值
    "KickOnProjectileThresholdBroken": false,  // 投射物阈值破坏踢出
    "HealOtherThreshold": 1050,  // 治愈他人阈值
    "KickOnHealOtherThresholdBroken": false,  // 治愈他人阈值破坏踢出
    "SuppressPermissionFailureNotices": false,  // 抑制权限失败通知
    "DisableModifiedZenith": false,  // 禁用修改过的巅峰
    "DisableCustomDeathMessages": true,  // 禁用自定义死亡消息
    "CommandSpecifier": "/",  // 命令指示符
    "CommandSilentSpecifier": ".",  // 静默命令指示符
    "DisableSpewLogs": true,  // 禁用喷射日志
    "DisableSecondUpdateLogs": false,  // 禁用第二次更新日志
    "SuperAdminChatRGB": [
      255,
      255,
      255
    ],
    "SuperAdminChatPrefix": "(超级管理员) ",
    "SuperAdminChatSuffix": "",  // 超级管理员聊天前缀和后缀
    "EnableGeoIP": false,  // 启用地理IP
    "DisplayIPToAdmins": false,  // 向管理员显示IP
    "ChatFormat": "{1}{2}{3}: {4}",  // 聊天格式
    "ChatAboveHeadsFormat": "{2}",  // 头顶聊天格式
    "EnableChatAboveHeads": false,  // 启用头顶聊天
    "BroadcastRGB": [
      127,
      255,
      212
    ],
    "StorageType": "sqlite",  // 存储类型
    "SqliteDBPath": "tshock.sqlite",  // SQLite数据库路径
    "MySqlHost": "localhost:3306",  // MySQL主机
    "MySqlDbName": "",  // MySQL数据库名称
    "MySqlUsername": "",  // MySQL用户名
    "MySqlPassword": "",  // MySQL密码
    "UseSqlLogs": false,  // 使用SQL日志
    "RevertToTextLogsOnSqlFailures": 10,  // SQL失败后返回文本日志
    "RestApiEnabled": false,  // 启用REST API
    "RestApiPort": 7878,  // REST API端口
    "LogRest": false,  // 记录REST请求
    "EnableTokenEndpointAuthentication": false,  // 启用令牌端点身份验证
    "RESTMaximumRequestsPerInterval": 5,  // 每个间隔的最大REST请求
    "RESTRequestBucketDecreaseIntervalMinutes": 1,  // REST请求桶减少间隔（分钟）
    "ApplicationRestTokens": {}  // 应用REST令牌
  }
}
```

```sscconfig.json
{
  "Settings": {
    "Enabled": true,  // 强制开荒启用
    "ServerSideCharacterSave": 15,  // 服务器端角色保存
    "LogonDiscardThreshold": 250,  // 登录丢弃阈值
    "StartingHealth": 100,  // 初始生命值
    "StartingMana": 20,  // 初始法力值
    "StartingInventory": [  // 初始物品清单
      {
        "netID": -15,  // 物品ID
        "prefix": 0,  // 前缀
        "stack": 1  // 堆叠数量
      },
      {
        "netID": -13,  // 物品ID
        "prefix": 0,  // 前缀
        "stack": 1  // 堆叠数量
      },
      {
        "netID": -16,  // 物品ID
        "prefix": 0,  // 前缀
        "stack": 1  // 堆叠数量
      }
    ],
    "WarnPlayersAboutBypassPermission": true  // 警告玩家绕过权限
  }
}
```

### 启动服务器

```bash
docker run -it --rm -p 7777:7777 -v $HOME/terraria/world:/root/.local/share/Terraria/Worlds --name terraria ryshe/terraria:latest
```

对于我来说是

```bash
docker run -it --rm -p 7777:7777 -v /var/lib/docker/volumes/terraria/_data/terraria/world:/root/.local/share/Terraria/Worlds --name terraria ryshe/terraria:latest
```

选择你作为服务器的世界。根据提示，开启泰拉瑞亚服务器。

![](https://img.0pt.im/game/built-tr/1-4.png)

现在来到服务器后台了。

# 管理（此部分较为简陋)

在服务器后台，先给default组加权限。（default组是登录后玩家默认所在的组。）

```
/group addperm default * !tshock.ignore.ssc
```

![](https://img.0pt.im/game/built-tr/1-5.png)

然后可以在服务端开账号。

```
/user add name passwd default
```

name是你的名字，也是你的账号。passwd是你的密码。default是你所在组。

管理后关闭移除容器。

# 后台运行

配置权限后， 将terraria后台运行

```bash
docker run -d -p 7777:7777 -v $HOME/terraria/world:/root/.local/share/Terraria/Worlds --name terraria -e WORLD_FILENAME=your.wld ryshe/terraria:latest
```

对于我来说，是

```bash
docker run -d -p 7777:7777 -v /var/lib/docker/volumes/terraria/_data/terraria/world:/root/.local/share/Terraria/Worlds --name terraria -e WORLD_FILENAME=tuguige.wld ryshe/terraria:latest
```

# 注册登录

打开游戏，连接你的泰拉瑞亚服务器。如果是搭建在本地。ip是 `localhost` 端口 `7777`

如果搭建在服务器，ip是你的服务器ip，端口7777，记得给服务器放行7777端口。

创建一个角色，角色名就是你的账号。

连接上服务器时，打开聊天栏，输`/help`获得帮助。

如果没有账号，先注册一个 ，输`/register passwd`

passwd是你的密码。你的账号是用户名。

然后再`/login passwd`。

完成。

# 后记

对管理部分的参考。

https://www.taozi1.com/16620.html

https://ikebukuro.tshock.co/
