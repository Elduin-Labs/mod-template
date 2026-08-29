# <MOD_DISPLAY_NAME>

<One plain sentence: what this mod does, in Elduin's words.>

This file is read automatically whenever Claude Code is opened in this folder.
Everything below is specific to this one mod. The general rules about how to
work with Elduin live in `~/.claude/CLAUDE.md`.

## Facts about this mod

    mod id            <mod_id>              (underscores — never change this)
    slug              <mod-slug>            (repo name and Modrinth slug)
    package           <com.elduin.mod_id>
    loader            <fabric>
    minecraft         <1.21.1, 1.21.4>
    primary version   <1.21.4>              (the one he plays)
    java              21

The mod id is baked into save files. Once a world has been played with this mod,
**changing the mod id breaks that world.** Rename the display name freely;
never rename the mod id.

## Layout

Multi-version is handled by [Stonecutter](https://plugins.gradle.org/plugin/dev.kikugie.stonecutter):
one source tree, version-conditional comments, many outputs. The version list
lives in `settings.gradle.kts`.

    src/main/java/<package>/       the mod
    src/main/resources/            fabric.mod.json, assets, textures
    versions/<mcversion>/build/libs/   built jars land here
    .github/workflows/release.yml  builds and publishes on a version tag

Do **not** add a branch or a repo for a new Minecraft version. Add it to the
Stonecutter list in `settings.gradle.kts` and fix whatever stops compiling.

## Commands

    ./gradlew build                    build every version
    ./gradlew "Set active project to 1.21.4"   switch the active version for the IDE

## Conventions for this repo

- Textures are 16x16 unless there's a reason. Keep the pixel-art style consistent
  with the rest of the mod.
- Every new block, item and mob needs an entry in the language file
  (`assets/<mod_id>/lang/en_us.json`) or it shows up in-game as a raw id, which
  reads to him as "broken".
- Anything a player can tune goes in the config, not hardcoded.
- Keep it dependency-free where possible. If a library is genuinely needed, it
  has to be one that's available for every Minecraft version in the list above.

## Releasing

Handled by the **share-it** skill. Short version: bump `mod_version` in
`gradle.properties`, update `CHANGELOG.md` in plain words, push a `v<version>`
tag, and the workflow publishes to Modrinth using the org's `MODRINTH_TOKEN`.
