# Red Prince Archipelago

## Project Repositories

- **[Releases and Setup](https://github.com/Tincancrafter/Red-Prince-Releases)** (you are here)
- [APWorld Source](https://github.com/Tincancrafter/Red-Prince-APWorld)
- [Game Mod Source](https://github.com/Tincancrafter/Red-Prince-Game-Mod)

This project adds Archipelago randomizer support to the Steam version of Blue
Prince. The game mod connects directly to an Archipelago server, sends checks
when the player finds randomized rewards, and applies received rooms, items,
upgrades, and effects in-game.

Ready-to-use downloads for both sides of a game are published on this
repository's [Releases page](https://github.com/Tincancrafter/Red-Prince-Releases/releases):

| File | Use |
| --- | --- |
| `RedPrinceArchipelago-1.0.3.zip` | Install the BepInEx game plugin. Every player needs this. |
| `redprince.apworld` | The custom Archipelago world, for hosts using an existing Archipelago install. |
| `RedPrince.yaml` | Player settings used to generate a seed. |

The mod is currently intended for Windows and the Steam release of Blue
Prince. Keep the plugin version, AP world, and generated seed from the same
release together.

## Player Installation

### Requirements

- Blue Prince installed on a Windows PC through Steam. Console versions are not
    supported by this PC BepInEx mod.
- Windows 10 or newer.
- BepInEx 6 IL2CPP, build `755` or a compatible newer setup.
- A modded Blue Prince client is fine. You may keep other BepInEx plugins
    installed; this mod does not require a clean or vanilla game installation.
- An Archipelago server address, your slot name, and the server password if the
  host configured one.

### Install BepInEx

You can complete this installation with a compatible mod manager instead of
copying files by hand. Create or select a profile for the **PC version of Blue
Prince**, install the BepInEx 6 IL2CPP Windows x64 package, and install the
`RedPrinceArchipelago-1.0.3.zip` plugin package through the manager. If the
manager asks where to install the plugin, its final location must be:

```text
<Blue Prince>\BepInEx\plugins\RedPrinceArchipelago
```

The manager must deploy the plugin DLLs directly into that folder. If it
creates a versioned folder such as `RedPrinceArchipelago-1.0.3` or a nested
folder, rename or move the package contents so the final folder is exactly
`RedPrinceArchipelago`. Use the manager's **Launch** button when starting the
game so it loads BepInEx and this profile's plugins. The manual steps below are
provided for managers that do not support importing this ZIP directly.

1. Close Blue Prince.
2. Download the BepInEx 6 Unity IL2CPP Windows x64 build. The known working
    build for this release is
    `BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.755+3fab71a.zip` from the
    [BepInEx build archive](https://builds.bepinex.dev/projects/bepinex_be).
3. Find the Blue Prince installation directory. In Steam, right-click Blue
    Prince, select **Manage**, then **Browse local files**. The default Windows
    path is usually:

    ```text
    C:\Program Files (x86)\Steam\steamapps\common\Blue Prince
    ```

4. Extract the contents of that archive directly into the Blue Prince
    installation directory. After extraction, `doorstop_config.ini` and the
    `BepInEx` folder should be directly under the game folder, not inside an
    extra folder such as `BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.755`.
5. Start Blue Prince once and wait for BepInEx to finish its first-run setup.
    Close the game afterward.

### Install the Red Prince Archipelago plugin

1. Download `RedPrinceArchipelago-1.0.3.zip` from this repository's release or
    download location.
2. Open the archive. Extract all of its contents into this exact folder:

    ```text
    <Blue Prince>\BepInEx\plugins\RedPrinceArchipelago
    ```

    Replace `<Blue Prince>` with the full installation path from the previous
    step. For example:

    ```text
    C:\Program Files (x86)\Steam\steamapps\common\Blue Prince\BepInEx\plugins\RedPrinceArchipelago
    ```

    Create the `RedPrinceArchipelago` folder if it does not exist. If the
    extracted folder has another name, rename it to exactly
    `RedPrinceArchipelago` before starting the game. Use that exact folder name
    for clarity and consistency; it must not be `RedPrinceArchipelago-1.0.3`,
    `RedPrinceArchipelago-main`, or a second nested
    `RedPrinceArchipelago` folder.

    Do not put the ZIP itself in the plugins folder. The DLL files must be
    directly inside the renamed folder, not one level deeper.
3. The folder should contain `RedPrinceArchipelago.dll`,
    `Archipelago.MultiClient.Net.dll`, `Newtonsoft.Json.dll`, and the `SessionData`
    folder.

If you already use other Blue Prince mods, leave them installed and place this
plugin beside them in `BepInEx\plugins`. Mod conflicts are possible, especially
for mods that change room drafting, inventories, saves, or the same user
interface. If the game crashes or the plugin does not load, temporarily disable
the other plugins one at a time to identify a conflict; do not delete your
existing mods or save files while troubleshooting.

### Connect in-game

1. Start Blue Prince and load or begin the save file that will be used for the
    Archipelago run.
2. Press `/` to open the mod console. Press `Escape` to hide it later.
3. Enter the following values:

    - **Host:** the address supplied by the host, such as `archipelago.gg:12345`
    - **Player Name:** the exact slot name from the generated YAML, usually
      `RedPrince`
    - **Password:** the server password, or leave it empty if there is none

4. Select **Connect**. A successful connection shows `Status: Connected`.
5. Keep the console available when troubleshooting. It displays Archipelago
    messages, received items, and connection errors.

The game client logs in as the Archipelago game `Red Prince`. A login failure
usually means the host is using a different game name, the slot name is not an
exact match, the password is wrong, or the client and server do not agree on
the installed world/version.

## Host Setup

The host needs an Archipelago installation that supports custom `.apworld`
files. A normal Archipelago release or a hosting provider that explicitly
allows custom worlds is required.

1. Download `redprince.apworld` and `RedPrince.yaml` from the same release.

2. Install the world by copying `redprince.apworld` into the `custom_worlds`
    folder of the Archipelago installation. If the host uses a web service,
    upload the `.apworld` through that service's custom-world workflow instead.
3. Copy `RedPrince.yaml` into the host's `Players` folder.
4. Edit the YAML before generating if you want different settings. The player
    name must remain the same name that the player enters in-game.
5. Generate the seed with Archipelago's normal Generate/Launcher workflow and
    start the resulting server. The host's Archipelago version must be
    compatible with the `.apworld` release.
6. Give each player the server address, port, slot name, and password. The
    player does not need the host's YAML after generation, but they do need the
    matching game plugin.

For a single-player test, the supplied YAML is already configured with:

- Game: `Red Prince`
- Slot name: `RedPrince`
- Goal: `room46`
- Room drafting, standard items, workshop items, upgrade disks, and keys
  randomized
- Traps and Death Link disabled

## YAML Options

The main settings in `RedPrince.yaml` are:

- `goal_type`: `antechamber`, `room46`, `sanctum`, `ascend`, or `blueprints`.
- `goal_sanctum_solves`: required Sanctum solves when using the `sanctum` goal,
  from 1 to 8.
- `room_draft_sanity`: randomizes the room draft when `true`.
- `standard_item_sanity`, `workshop_sanity`, `upgrade_disk_sanity`, and
  `key_sanity`: enable the corresponding check groups.
- `item_logic_mode`: `default`, `rare`, `complex`, `rare_complex`, or `extreme`.
- `special_shop_sanity` and `trophy_sanity`: add shop and trophy checks.
- `death_link_type`: `none`, `eod`, `bedroom`, or `steps`.

Use the comments in the supplied YAML for ranges and the complete list of
room/trunk and filler settings. Do not change the top-level `game` value or
the player name after a seed has been generated.

## Troubleshooting

**The mod does not appear:** confirm BepInEx was extracted into the game folder,
then confirm the plugin DLL is directly inside the exact path
`<Blue Prince>\BepInEx\plugins\RedPrinceArchipelago`. If the folder has a
version suffix or `-main` suffix, rename it to exactly `RedPrinceArchipelago`.

**Another mod causes a crash or missing features:** temporarily disable that
mod and test again. This Archipelago plugin can coexist with a modded client,
but two mods that patch the same game systems may conflict.

**The console does not open:** enter a game and press `/`, not `Escape` or the
Steam overlay shortcut. Check `BepInEx\LogOutput.log` for plugin-loading errors.

**Login fails:** check the host address and password, use the exact YAML slot
name, and confirm the host installed the same `redprince.apworld` release.

**Items or checks are missing:** stop the run and verify that the plugin and
world were not mixed between releases. Reconnect to the original server and
use the in-game `/help` command to see available client commands.

**A third-party host refuses the world:** custom `.apworld` uploads are not
supported by every Archipelago host. Use a host that advertises custom-world
support or run the Archipelago server locally.

## Source Repositories

- [Red-Prince-APWorld](https://github.com/Tincancrafter/Red-Prince-APWorld)
    contains the Archipelago Python world, tests, and world documentation.
- [Red-Prince-Game-Mod](https://github.com/Tincancrafter/Red-Prince-Game-Mod)
    contains the BepInEx C# plugin, assets, solution, and build documentation.
- This repository contains player-facing installation and hosting documentation.
    Compiled game files are attached to releases rather than committed to the branch.

## Credits and Sources

### Project Credits

Thank you to everyone who contributed code, research, art, documentation,
testing, tooling, packaging, or advice:

- [Yascob99](https://github.com/Yascob99), repository owner and lead developer
    of the Blue Prince game mod.
- [BatmenzDW](https://github.com/BatmenzDW), primary APWorld author and a major
    contributor to the game mod.
- [deefdragon](https://github.com/deefdragon), for APWorld development, logic,
    and integration work.
- [shavnir](https://github.com/shavnir), for game-mod code, build tooling,
    item/list handling, and installation documentation.
- [Rooby-Roo](https://github.com/Rooby-Roo), for documentation contributions.
- ChaseoQueso, for the initial item code and custom Archipelago swirl asset.
- Mac, for work on the mod and APWorld.
- Zygan, for custom art assets.
- [Emmet-is-a-Birb](https://github.com/Emmet-is-a-Birb), author of the related
    standalone [Blue Prince Manual integration](https://github.com/Emmet-is-a-Birb/BluePrince_Manual).
- The Silksong and Hollow Knight communities, for modding tools and research
    that made parts of this project possible.

### Software and Documentation Sources

This project builds on or refers to the following software and resources:

- [Archipelago](https://archipelago.gg/), for the multiworld randomizer,
    server workflow, world format, and client protocol.
- [Archipelago MultiClient.Net](https://github.com/ArchipelagoMW/Archipelago),
    used by the game plugin to connect to Archipelago servers.
- [BepInEx 6](https://docs.bepinex.dev/), including the
    [Unity IL2CPP build archive](https://builds.bepinex.dev/projects/bepinex_be),
    for loading the plugin into Blue Prince.
- [AssetRipper](https://github.com/AssetRipper/AssetRipper), used during Unity
    asset and project investigation.
- [Cinematic Unity Explorer](https://github.com/asd9176506911298/CinematicUnityExplorer),
    referenced as an optional Unity investigation tool in the plugin documentation.
- [Unity](https://unity.com/), [Unity Hub](https://unity.com/download), and
    the Blue Prince game on Steam, as the game and development environment.
- The [BepInEx NuGet feed](https://nuget.bepinex.dev/v3/index.json) and
    [Samboy NuGet feed](https://nuget.samboy.dev/v3/index.json), used by the C#
    development setup.

Links and software versions may change. The version details used for the
current release are recorded in the
[game-mod project file](https://github.com/Tincancrafter/Red-Prince-Game-Mod/blob/main/RedPrinceArchipelago/RedPrinceArchipelago.csproj).

### AI Assistance Disclosure

[GitHub Copilot](https://github.com/features/copilot), an AI coding assistant,
was used to help organize and expand this README, inspect the repository,
draft installation and troubleshooting instructions, and validate the Git
ignore configuration. The project authors remain responsible for reviewing,
testing, and correcting all generated or suggested content. AI assistance does
not replace credit for the human contributors or the upstream projects listed
above.

## License

This project is distributed under the [MIT License](LICENSE).
