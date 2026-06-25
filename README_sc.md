# WorldNamePacket

[English](README.md) | [中文](README_sc.md)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/Y8Y726QMH)

为客户端小地图模组提供支持的**服务端**辅助模组。在多世界环境下自动设置世界名称，
告别手动配置和"无法识别世界"的报错。

**平台：** 仅 Fabric（MC 1.21.4+）  
**支持的小地图：** VoxelMap、Xaero's World Map（萨罗地图）

> **注意：** v2.0 已移除对 Spigot/Paper 和 Velocity 的支持。  
> Bukkit 系服务端请使用 [AdvancedServerFlags](https://modrinth.com/plugin/advancedserverflags) 等仍在维护的替代方案。

## 功能

当你连接的服务器拥有超过原版 3 个维度时（常见于模组服，或 BungeeCord/Velocity
等群组代理后端），小地图模组容易混淆各个世界的地图数据，甚至反复弹出"你当前在
哪个世界？"的询问。

WorldNamePacket 通过在玩家加入/切换世界时向小地图模组发送正确的世界名称来解决此问题。

| 小地图模组 | 通信频道 | 机制 |
|-----------|---------|------|
| VoxelMap | `worldinfo:world_id` | 请求 → 响应 |
| Xaero's World Map | `xaeroworldmap:main` | 服务器主动推送 |

## 安装

1. 从 [Releases](https://github.com/kosma/worldnamepacket/releases) 下载 jar 文件。
2. 放入服务端的 `mods/` 文件夹。
3. 需要 [Fabric Loader](https://fabricmc.cn/) ≥ 0.16 和 [Fabric API](https://modrinth.com/mod/fabric-api)。
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

## 许可证

MIT — 详见 [LICENSE](LICENSE)。
