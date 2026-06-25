# WorldNamePacket

[English](README.md) | [中文](README_sc.md)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/Y8Y726QMH)

Server-side companion mod for client-side mapping mods. Automatically sets the
world name in multi-world setups — no more manual configuration and "world not
recognized" errors.

**Platform:** Fabric only (MC 1.21.4+)  
**Supported map mods:** VoxelMap, Xaero's World Map

> **Note:** Spigot/Paper and Velocity support has been removed in v2.0.  
> For Bukkit-based servers, use [AdvancedServerFlags](https://modrinth.com/plugin/advancedserverflags) or similar maintained alternatives.

## Functionality

When you connect to a server with more than the 3 vanilla dimensions (common on
modded servers, or proxy setups like BungeeCord/Velocity), mapping mods can get
confused — mixing up maps or asking "which world are you on?"

WorldNamePacket solves this by telling the mapping mod the correct world name
on every world join or dimension change.

| Map Mod | Channel | Mechanism |
|---------|---------|-----------|
| VoxelMap | `worldinfo:world_id` | Request → Response |
| Xaero's World Map | `xaeroworldmap:main` | Push on world change |

## Installation

1. Download the jar from [Releases](https://github.com/kosma/worldnamepacket/releases).
2. Place it in your server's `mods/` folder.
3. Requires [Fabric Loader](https://fabricmc.net/) ≥ 0.16 and [Fabric API](https://modrinth.com/mod/fabric-api).
4. No client-side installation needed.

## Configuration

There is nothing to configure. The mod reads the world name from your server's
`level-name` in `server.properties`. Make sure each server/world has a unique
`level-name` — the default `world` won't help distinguish dimensions.

## For Developers

### Building

```bash
./gradlew build
```

Requires JDK 21+.

### Upgrading to a new Minecraft version

Edit `gradle.properties`:

```properties
minecraft_version=<target>
yarn_mappings=<target>+build.1
fabric_version=<matching fabric api>
```

Then update `fabric.mod.json` → `depends.minecraft` accordingly.

### How it works

The mod uses a Mixin to hook into `PlayerManager.sendWorldInfo()`, which fires
on player join and dimension change. It then sends the world name via Fabric
networking (CustomPayload API) on the appropriate plugin channel.

## License

MIT — see [LICENSE](LICENSE).
