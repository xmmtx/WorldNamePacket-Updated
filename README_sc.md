# WorldNamePacket-Updated
*[WorldNamePacket-Updated](https://github.com/xmmtx/WorldNamePacket-Updated)是[WorldNamePacket](https://github.com/kosmolot-mods/worldnamepacket)的高版本分支*

<div align="center">
  <a href="./LICENSE">
    <img src="https://img.shields.io/github/license/xmmtx/WorldNamePacket-Updated" alt="License" />
  </a>
  <a href="https://www.xm233.cn/sponsor">
    <img src="https://img.shields.io/badge/%24-sponsor-F87171.svg" alt="sponsor" />
  </a>
</div>

[English](./README.md) | 中文

## 简介
服务器端地图模组助手。在多世界模式下自动设置世界名称——告别手动配置和“世界未识别”的提示。  
现仅支持`Fabric`加载器，还有`VoxelMap`、`Xaero's Map`这两个地图Mod。

## 功能
这个模组可以解决一个特定问题：当你连接到一个拥有超过原版游戏三维度的服务器时，地图模组很容易出错，要么会把地图搞混，要么会询问你当前所在的地图。这种情况会发生在许多安装了模组的服务器上，但如果原版服务器使用了代理服务器（例如 Bungeecord/Velocity 等），也会出现这种情况。

这个工具通过告诉地图模组它连接到了哪个世界来解决这个问题。

## 安装
1. 从 [Releases](https://github.com/xmmtx/WorldNamePacket-Updated/releases) 下载 jar 文件。
2. 放入服务端的 `mods` 文件夹。
3. 需要 [Fabric Loader](https://fabricmc.net) ≥ 0.16 和 [Fabric API](https://modrinth.com/mod/fabric-api)。
4. 客户端无需安装。

## 配置
无需任何配置。模组会自动读取 `server.properties` 中的 `level-name` 作为世界名。
请确保每个服务器/世界的 `level-name` 各不相同——默认的 `world` 无法区分不同维度。

## 开发者指南
### 构建

```bash
./gradlew build
```

需要 JDK 21+。

### 升级到新 Minecraft 版本
编辑 `gradle.properties`：

```properties
minecraft_version=<目标版本>
yarn_mappings=<目标版本>+build.1
fabric_version=<对应的 Fabric API 版本>
```

然后同步更新 `fabric.mod.json` 中的 `depends.minecraft`。

### 工作原理
通过 Mixin 注入 `PlayerManager.sendWorldInfo()` 方法，在玩家加入或切换世界时触发，
然后使用 Fabric 网络 API（CustomPayload）在对应用频道上发送世界名称。
