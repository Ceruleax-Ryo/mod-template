> **This repository is a template.** See [TEMPLATE.md](TEMPLATE.md) for how to start a new mod
> from it, then delete this note and `TEMPLATE.md`.

# Template

One or two sentences: what the mod does and who it is for.

## Features

- What it changes, stated as behaviour a player or server owner can observe.

## Compatibility

| | |
|---|---|
| Minecraft | 26.3 |
| Mod loader | Fabric (Fabric Loader 0.20.1 or newer) |
| Side | Server. Not needed on clients that join a server; install it on the client to use it in singleplayer. |
| Dependencies | None |

Known interactions with other mods:

- **Lithium** – not yet tested.
- **Carpet** – not yet tested.

## Installation

Download the jar that matches your Minecraft version from
[Modrinth](https://modrinth.com/) or [CurseForge](https://www.curseforge.com/minecraft/mc-mods)
and put it in the `mods` folder.

## Building from source

Requires JDK 25.

```
./gradlew build
```

The jars are written to `versions/<minecraft>-<loader>/build/libs/`.

## License

[MIT](LICENSE)
