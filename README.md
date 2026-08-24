# Blue Prince Archipelago

This project adds Archipelago randomizer support to the Steam version of Blue
Prince. The game mod connects directly to an Archipelago server, sends checks
when the player finds randomized rewards, and applies received rooms, items,
upgrades, and effects in-game.

This repository includes ready-to-use downloads for both sides of a game:

| File | Use |
| --- | --- |
| `BluePrinceArchipelago-1.0.3.zip` | Install the BepInEx game plugin. Every player needs this. |
| `blue-prince-host-bundle-fixed.zip` | Give to the host. Contains the AP world and a sample player YAML. |
| `blueprince.apworld` | The custom Archipelago world, for hosts using an existing Archipelago install. |
| `Players/BluePrince.yaml` | Player settings used to generate a seed. |

The mod is currently intended for Windows and the Steam release of Blue
Prince. Keep the plugin version, AP world, and generated seed from the same
release together.

## Player Installation

### Requirements

- Blue Prince installed through Steam.
- Windows 10 or newer.
- BepInEx 6 IL2CPP, build `755` or a compatible newer setup.
- An Archipelago server address, your slot name, and the server password if the
  host configured one.

### Install BepInEx

1. Close Blue Prince.
2. Download the BepInEx 6 Unity IL2CPP Windows x64 build. The known working
    build for this release is
    `BepInEx-Unity.IL2CPP-win-x64-6.0.0-be.755+3fab71a.zip` from the
    [BepInEx build archive](https://builds.bepinex.dev/projects/bepinex_be).
3. Extract the contents of that archive into the Blue Prince installation
    directory, usually:

    ```text
    C:\Program Files (x86)\Steam\steamapps\common\Blue Prince
    ```

4. Start Blue Prince once and wait for BepInEx to finish its first-run setup.
    Close the game afterward.

### Install the Blue Prince plugin

1. Download `BluePrinceArchipelago-1.0.3.zip` from this repository's release or
    download location.
2. Open the archive. Extract all of its contents into:

    ```text
    <Blue Prince>\BepInEx\plugins\BluePrinceArchipelago
    ```

    Create the `BluePrinceArchipelago` folder if it does not exist. Do not put
    the ZIP itself in the plugins folder and do not nest another copy of the
    folder inside it.
3. The folder should contain `BluePrinceArchipelago.dll`,
    `Archipelago.MultiClient.Net.dll`, `Newtonsoft.Json.dll`, and the `SessionData`
    folder.

### Connect in-game

1. Start Blue Prince and load or begin the save file that will be used for the
    Archipelago run.
2. Press `/` to open the mod console. Press `Escape` to hide it later.
3. Enter the following values:

    - **Host:** the address supplied by the host, such as `archipelago.gg:12345`
    - **Player Name:** the exact slot name from the generated YAML, usually
      `BluePrince`
    - **Password:** the server password, or leave it empty if there is none

4. Select **Connect**. A successful connection shows `Status: Connected`.
5. Keep the console available when troubleshooting. It displays Archipelago
    messages, received items, and connection errors.

The game client logs in as the Archipelago game `Blue Prince`. A login failure
usually means the host is using a different game name, the slot name is not an
exact match, the password is wrong, or the client and server do not agree on
the installed world/version.

## Host Setup

The host needs an Archipelago installation that supports custom `.apworld`
files. A normal Archipelago release or a hosting provider that explicitly
allows custom worlds is required.

1. Download `blue-prince-host-bundle-fixed.zip` and extract it into a temporary
    folder. It contains:

    ```text
    blueprince.apworld
    Players\BluePrince.yaml
    ```

2. Install the world by copying `blueprince.apworld` into the `custom_worlds`
    folder of the Archipelago installation. If the host uses a web service,
    upload the `.apworld` through that service's custom-world workflow instead.
3. Copy `Players/BluePrince.yaml` into the host's `Players` folder.
4. Edit the YAML before generating if you want different settings. The player
    name must remain the same name that the player enters in-game.
5. Generate the seed with Archipelago's normal Generate/Launcher workflow and
    start the resulting server. The host's Archipelago version must be
    compatible with the `.apworld` release.
6. Give each player the server address, port, slot name, and password. The
    player does not need the host's YAML after generation, but they do need the
    matching game plugin.

For a single-player test, the supplied YAML is already configured with:

- Game: `Blue Prince`
- Slot name: `BluePrince`
- Goal: `room46`
- Room drafting, standard items, workshop items, upgrade disks, and keys
  randomized
- Traps and Death Link disabled

## YAML Options

The main settings in `Players/BluePrince.yaml` are:

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
then confirm the plugin DLL is directly inside
`BepInEx\plugins\BluePrinceArchipelago`.

**The console does not open:** enter a game and press `/`, not `Escape` or the
Steam overlay shortcut. Check `BepInEx\LogOutput.log` for plugin-loading errors.

**Login fails:** check the host address and password, use the exact YAML slot
name, and confirm the host installed the same `blueprince.apworld` release.

**Items or checks are missing:** stop the run and verify that the plugin and
world were not mixed between releases. Reconnect to the original server and
use the in-game `/help` command to see available client commands.

**A third-party host refuses the world:** custom `.apworld` uploads are not
supported by every Archipelago host. Use a host that advertises custom-world
support or run the Archipelago server locally.

## Development

The Python module in `blueprince_functions.py` is a dependency-free rules layer
used to exercise the world logic. Run its tests with:

```text
python -m unittest -v
```

The C# project is under `blueprince-plugin/`. Building it requires the Blue
Prince installation path and the BepInEx/Archipelago dependencies described in
that project's README. The source world used for development is under
`archipelago-upstream/worlds/blueprince/`.

## Credits and License

See the plugin README and `blueprince-plugin/LISCENSE.md` for project credits
and license information.