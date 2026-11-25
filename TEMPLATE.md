# Using this template

Shared starting point for Ceruleax's Minecraft mods: Fabric, Minecraft 26.3, Java 25,
built with [Stonecutter](https://stonecutter.kikugie.dev/) so that more Minecraft versions
and NeoForge can be added later without restructuring.

Delete this file once the new mod is set up.

## Starting a new mod

1. Create the repository from this template (`gh repo create <name> --template Ceruleax-Ryo/mod-template`)
   or copy the folder.
2. In `stonecutter.properties.toml`, set `mod.id`, `mod.name`, `mod.description`,
   `mod.repository` and `mod.side`.
3. In `settings.gradle.kts`, set `rootProject.name`.
4. Rename the `template` identifiers to the new mod id:
   - package `com.ceruleax.template` and the class `TemplateMod` (including its `MOD_ID`)
   - `src/main/resources/template.mixins.json`, and the `package` inside it
   - the `entrypoints` and `mixins` entries in `src/main/resources/fabric.mod.json`
   - the `template$` member prefix mentioned in `mixin/package-info.java`
5. Rewrite `README.md` and remove the template note at its top.
6. Run `./gradlew build`, then `./gradlew :26.3-fabric:runClient` to check it loads.

A client-only mod sets `mod.side = "client"`, registers its mixins under `"client"` in the
mixin config, and replaces the `main` entrypoint with a `client` one (`ClientModInitializer`).

## Layout

| Path | Purpose |
|---|---|
| `stonecutter.properties.toml` | Mod metadata, dependency versions and per-version values. The file to edit most often. |
| `settings.gradle.kts` | The list of nodes (Minecraft version × loader). |
| `build.fabric.gradle.kts` | Build and publishing logic shared by all Fabric nodes. |
| `stonecutter.gradle.kts` | Stonecutter setup: property tags and the `fabric` / `neoforge` source constants. |
| `src/` | The single source tree, shared by every node. |
| `versions/<node>/` | Per-node build output. Nothing here is checked in. |

Fabric API is not a dependency. Add it to a mod only when that mod needs it.

## Adding a Minecraft version

1. Add `match("<version>", "fabric")` in `settings.gradle.kts`.
2. Add the `["<version>"]` and `[fabric."<version>"]` tables in `stonecutter.properties.toml`.
3. Where the source has to differ between versions, use
   [Stonecutter conditions](https://stonecutter.kikugie.dev/wiki/v2/reference/syntax/conditions):

   ```java
   //? if >=26.4 {
   newCall();
   //?} else {
   /*oldCall();
   *///?}
   ```

`./gradlew build` builds every node. To edit and run the code as a different version, run the
`Set active project to <node>` Gradle task first; switch back to the `vcsVersion` node before
committing.

Versions before 26.1 are obfuscated and need a different Loom plugin; see
[loom-back-compat](https://stonecutter.kikugie.dev/wiki/v2/guides/tips/loom-back-compat).

## Releasing

One-time setup per mod:

- Create the projects on Modrinth and CurseForge and put their IDs in
  `publish.modrinth_id` and `publish.curseforge_id`. A platform with an empty ID is skipped.
- Add the repository secrets `MODRINTH_TOKEN` (scopes: create, read and write versions) and
  `CURSEFORGE_TOKEN`.

For each release:

1. Set `mod.version` in `stonecutter.properties.toml`.
2. In `CHANGELOG.md`, move the entries under `[Unreleased]` to a new `## [<version>] - <date>` section.
3. Commit, then tag and push: `git tag v<version>` and `git push origin main v<version>`.

The release workflow checks that the tag matches `mod.version` and that the changelog has a
section for it, builds every node, uploads each jar to Modrinth and CurseForge, and creates a
GitHub release with the same notes.

A version containing `alpha` or `beta` (for example `1.3.0-beta.1`) is published with that
release type and marked as a pre-release on GitHub.

To see what would be uploaded without uploading anything, run `./gradlew publishMods` locally:
outside CI it is always a dry run.
