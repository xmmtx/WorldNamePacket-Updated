# WorldNamePacket-Updated
*[WorldNamePacket-Updated](https://github.com/xmmtx/WorldNamePacket-Updated) is the high-version fork of [WorldNamePacket](https://github.com/kosmolot-mods/worldnamepacket)*

<div align="center">
  <a href="./LICENSE">
    <img src="https://img.shields.io/github/license/xmmtx/WorldNamePacket-Updated" alt="License" />
  </a>
  <a href="https://www.xm233.cn/sponsor">
    <img src="https://img.shields.io/badge/%24-sponsor-F87171.svg" alt="sponsor" />
  </a>
</div>

English | [中文](./README_sc.md)

## Introduction
Server-side companion mod for client-side mapping mods. Automatically sets the world name in multi-world setups — no more manual configuration and "world not recognized" errors.  
Now only supports `Fabric` loader, along with `VoxelMap` and `Xaero's Map`.

## Functionality
This mod helps in a specific situation: where you connect to a server that has more than the 3 vanilla dimensions, mapping mods easily get confused and can either mix up the maps or ask you which one you're on. This can happen on many modded servers - but also vanilla servers if there are behind a proxy (Bungeecord/Velocity/etc).

This tool solves this problem by telling the mapping mod which world it's connected to.

## Installation
1. Download the jar from [Releases](https://github.com/xmmtx/WorldNamePacket-Updated/releases).
2. Place it in your server's `mods` folder.
3. Requires [Fabric Loader](https://fabricmc.net) ≥ 0.16 and [Fabric API](https://modrinth.com/mod/fabric-api).
4. No client-side installation needed.

## Configuration
There is nothing to configure. The mod reads the world name from your server's `level-name` in `server.properties`. Make sure each server/world has a unique `level-name` — the default `world` won't help distinguish dimensions.

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
fabric_version=<matching Fabric API>
```

Then update `fabric.mod.json` → `depends.minecraft` accordingly.

### How it works
The mod uses Mixin to hook into `PlayerManager.sendWorldInfo()`, which fires on player join and dimension change. It then sends the world name via the Fabric Networking API (CustomPayload) on the appropriate plugin channel.
