A guide on how to **download** and **install mods** in [Risk of Rain 2](https://store.steampowered.com/app/632360/Risk_of_Rain_2/) on PC, covering [r2modman](https://thunderstore.io/c/riskofrain2/p/ebkr/r2modman/), the manual method, and the dependency stack that trips most people up.

Risk of Rain 2 has one of the oldest and best organised modding scenes on [Thunderstore](https://thunderstore.io/c/riskofrain2/). It also has one of the deepest dependency stacks, and understanding that stack is genuinely the difference between a working install and an hour of confusion.

Our worked example is [Starstorm 2](https://thunderstore.io/c/riskofrain2/p/TeamMoonstorm/Starstorm2/), a large content mod that adds survivors, items and enemies. It is a good example precisely because it is not a simple one.

[**View Guide On TMC (Recommended Due To Better Formatting)**](https://moddingcommunity.com/blog/how-to-install-mods-in-risk-of-rain-2/)

## Table Of Contents
* [Requirements](#requirements)
* [The Mod Stack](#the-mod-stack)
    * [BepInEx](#bepinex)
    * [R2API](#r2api)
    * [The Mod Itself](#the-mod-itself)
* [Installing With r2modman](#installing-with-r2modman)
    * [Setting Up A Profile](#setting-up-a-profile)
    * [Installing Starstorm 2](#installing-starstorm-2)
    * [Launching Modded](#launching-modded)
* [Installing Manually](#installing-manually)
* [Installing With The TMC App](#installing-with-the-tmc-app)
* [Multiplayer And Mod Syncing](#multiplayer-and-mod-syncing)
* [Consoles](#consoles)
* [Managing Your Mods](#managing-your-mods)
* [Troubleshooting](#troubleshooting)
* [Conclusion](#conclusion)
* [See Also](#see-also)

## Requirements
* A PC running **Windows 10** or later, or **Linux**.
* **Risk of Rain 2** on Steam or the Epic Games Store.
* A few hundred MB free. Content mods on the scale of Starstorm 2 run larger.
* An archive tool such as [7-Zip](https://www.7-zip.org/) if you install by hand.

There is no anti-cheat in Risk of Rain 2 and no ban risk for modding it. Mods do disable Steam achievements in some configurations, though, so bear that in mind if you are still chasing them.

## The Mod Stack
Risk of Rain 2 mods sit on top of two layers, and a mod will refuse to load if either one is missing or out of date. This is worth understanding even if you use a mod manager that handles it for you, because it explains every error message you are likely to see.

### BepInEx
[BepInExPack](https://thunderstore.io/c/riskofrain2/p/bbepis/BepInExPack/) is the mod loader. It hooks the game at launch and loads plugins out of `BepInEx/plugins`.

The Risk of Rain 2 pack is not plain BepInEx. It ships several community patches bundled in, including `RoR2BepInExPack`, `FixPluginTypesSerialization` and `BepInEx_GUI`. Those exist because of quirks specific to this game, which is why you want this pack rather than upstream BepInEx.

### R2API
[R2API](https://thunderstore.io/c/riskofrain2/p/tristanmcpherson/R2API/) is a modding API that sits between BepInEx and the mods. It gives mod authors a stable, shared way to add survivors, items, elites, buffs and so on without every mod reinventing it and colliding with every other mod.

R2API was split into submodules a while back, and this is the part that confuses people. Rather than one big `R2API.dll`, there are now separate packages such as `R2API_Prefab`, `R2API_Networking` and `R2API_Difficulty`. A mod only depends on the submodules it actually uses. Starstorm 2 pulls in around a dozen of them.

You almost never install these by hand. Let the mod manager do it.

### The Mod Itself
With those two layers present, the mod is a `.dll` in `BepInEx/plugins` like any other BepInEx plugin.

**NOTE** - Versions matter here more than in most games. A mod built against R2API 5.0.5 may not load against an older R2API. Mod managers install the versions a mod declares, which is the main reason manual installs go wrong more often in this game than elsewhere.

## Installing With r2modman
r2modman is the standard tool for this game and was originally written for it. [Thunderstore Mod Manager](https://www.overwolf.com/app/Thunderstore-Thunderstore_Mod_Manager) is built on the same codebase and works identically if you prefer it; [Gale](https://thunderstore.io/c/riskofrain2/p/Kesomannen/GaleModManager/) is a lighter alternative that also supports this game.

### Setting Up A Profile
1. Download r2modman from its [Thunderstore page](https://thunderstore.io/c/riskofrain2/p/ebkr/r2modman/) or from [GitHub](https://github.com/ebkr/r2modmanPlus/releases).
2. Launch it and pick **Risk of Rain 2**.
3. Choose the store you own the game on, Steam or Epic. r2modman finds the install folder from there.
4. You will land on the profile screen. Create a new profile rather than using **Default**, and name it after what you are doing with it, for example `starstorm`.

Profiles are the feature worth learning. Each one keeps its own mod list, its own configs and its own save-adjacent state, and switching between them is instant and does not re-download anything. Keeping one profile per playthrough or per friend group is much less painful than enabling and disabling mods by hand.

### Installing Starstorm 2
1. With your profile selected, click **Online** in the left sidebar.
2. Search for **Starstorm2** and open the result by TeamMoonstorm.
3. Click **Download**, then **Download with dependencies** when prompted.
4. Let it run. It will pull in BepInExPack, MoonstormSharedUtils and the R2API submodules Starstorm 2 needs, which is a long list.
5. Switch to **Installed** and confirm everything shows up without a warning icon.

### Launching Modded
Click **Start modded** at the top of the r2modman window.

Do not launch the game from Steam or Epic. That starts it vanilla. r2modman works by injecting BepInEx at launch time through the profile, so the launch has to go through r2modman for any of it to apply.

**TIP** - If you want a Steam shortcut that launches modded, r2modman can generate the launch arguments for you under **Settings**, then **Browse profile folder**. Most people find it simpler to just keep r2modman open.

## Installing Manually
Doable, and worth knowing, but genuinely more tedious in this game than in most because of the R2API submodule situation.

Find your game folder first. In Steam, right-click **Risk of Rain 2**, then **Manage** and **Browse local files**:

```
C:\Program Files (x86)\Steam\steamapps\common\Risk of Rain 2
```

Then:

1. Download [BepInExPack](https://thunderstore.io/c/riskofrain2/p/bbepis/BepInExPack/) with **Manual Download**, extract it, and copy the **contents** of the `BepInExPack` folder into the game folder. You should end up with `BepInEx`, `doorstop_config.ini` and `winhttp.dll` sitting beside `Risk of Rain 2.exe`.
2. Launch the game once so BepInEx builds its folders, then quit.
3. Download [R2API](https://thunderstore.io/c/riskofrain2/p/tristanmcpherson/R2API/) and each submodule the mod lists, extract them, and copy their `.dll` files into `BepInEx/plugins`.
4. Download Starstorm 2 and copy its contents into `BepInEx/plugins` as well.

Step 3 is the catch. To do this properly you need to read the mod's **Dependencies** list on Thunderstore, fetch each entry at the exact version listed, and repeat for anything those have as dependencies. For Starstorm 2 that is over a dozen downloads.

**WARNING** - If you install manually, do not mix and match versions. Grabbing the newest R2API submodules when the mod asks for older ones is the most common cause of a mod silently failing to load.

## Installing With The TMC App
There is one more manager worth mentioning, which is our own. [The TMC App](https://moddingcommunity.com/tmc-app) does one-click mod installs, **sandboxes** (named profiles per game, each with its own load order and deployment method, switchable without re-downloading anything), a server browser with live latency graphs, and RCON.

**Risk of Rain 2 is not in its supported games list yet.** We would like it to be, and adding a game is a matter of four JSON files rather than code.

Before you go looking, though: **the app is in very early development** and says so itself. Most of it is only partially tested. For a game with a dependency graph as deep as this one, r2modman remains the tool to trust, and the TMC App is something to experiment with next to it rather than in place of it. Anyone willing to try it and report back is doing us a real favour.

It is **open source** under GPL-3.0 at [github.com/modcommunity/tmc-app](https://github.com/modcommunity/tmc-app). Issues and feature requests go in [the tracker](https://github.com/modcommunity/tmc-app/issues), pull requests are welcome, and the repository explains the per-game format if you want to add Risk of Rain 2 support yourself.

Installing it:

* **Linux**: one line, no root and no package manager.

```bash
curl -fsSL https://raw.githubusercontent.com/modcommunity/tmc-app/main/scripts/install.sh | sh
```

* **Windows**: the `setup.exe` or `setup.msi` from the [releases page](https://github.com/modcommunity/tmc-app/releases). There is a portable build too, though it does not register the launcher entry or the `tmc://` link handler.
* **macOS**: the `.dmg` from the same releases page.

## Multiplayer And Mod Syncing
Risk of Rain 2 multiplayer requires every player to be running compatible mods. Content mods that add items or survivors have to match across the lobby, and a mismatch normally shows up as a kick on join or as desynced items mid-run.

The practical solution is profile sharing. In r2modman, open your profile, click **Export**, and pick either **Export as file** or **Export as code**. Your friends use **Import** with the file or code and end up on a byte-identical mod list.

**NOTE** - Purely visual mods and some quality of life mods are client-side and do not need to match. The mod's Thunderstore page normally says so. When it does not say, assume it needs to match.

## Consoles
Risk of Rain 2 is also on PlayStation, Xbox and Nintendo Switch, and none of those versions support mods. Everything in this guide is PC only. There is no workaround, and anything claiming to mod the console builds is not something you should be downloading.

## Managing Your Mods
r2modman shows an update icon next to any installed mod that has a newer version on Thunderstore, and **Update all** handles the lot. Because R2API submodules are versioned independently, updating one mod will sometimes pull newer submodules along with it, which can break a second mod that wanted the old ones. This is exactly the situation profiles exist for: clone the profile, update the clone, and keep the working one until you are sure.

Disabling a mod is a toggle in the **Installed** list and leaves the files in place. Uninstalling removes them.

To go fully vanilla, delete `BepInEx`, `doorstop_config.ini` and `winhttp.dll` from the game folder. Verifying files through Steam will not remove them, since Steam does not track files it did not install.

## Troubleshooting
**Modded run, but no mods.** You launched from Steam or Epic instead of from r2modman.

**A mod is installed but never loads.** Check the BepInEx console for a line naming it. If the console mentions a missing type or method, it is a version mismatch against R2API. Reinstall the mod with dependencies from the manager.

**The game hangs on the splash screen.** Usually a mod built for an older game version. Risk of Rain 2 patches break mods regularly and the fix is normally to wait for the author.

**Kicked when joining a friend.** Mod lists do not match. Import their exported profile.

**Achievements stopped unlocking.** Expected behaviour with certain mods installed. Use a separate vanilla profile if you want to keep earning them.

**Linux and Proton.** Set your Steam launch options to `WINEDLLOVERRIDES="winhttp=n,b" %command%` so BepInEx gets loaded. r2modman handles this itself when you launch through it.

## Conclusion
Risk of Rain 2 modding looks intimidating from the outside because of the R2API submodules, but in practice a mod manager makes the whole thing a two-click operation. Install r2modman, make a profile, download with dependencies, and launch modded.

The two things worth actually remembering: launch through the manager rather than through Steam, and export your profile for whoever you play with.

If you have the time, the [TMC App](https://github.com/modcommunity/tmc-app) is open source, early in development, and feedback on it would be genuinely appreciated.

## See Also
* [Risk of Rain 2 on Thunderstore](https://thunderstore.io/c/riskofrain2/)
* [Risk of Rain 2 Modding Discord](https://discord.gg/5MbXZvd)
* [R2Wiki](https://github.com/risk-of-thunder/R2Wiki/wiki) - The community modding wiki, and the best reference for mod authors.
* [R2API on GitHub](https://github.com/risk-of-thunder/R2API)
* [TMC App](https://github.com/modcommunity/tmc-app)

We try to keep this guide current, but the game, BepInEx and R2API all update on their own schedules. If something here no longer lines up with what you are seeing, let us know or open a [pull request](https://github.com/modcommunity/how-to-install-mods-in-risk-of-rain-2/pulls) on this guide's GitHub repository.

Join our [Discord server](https://discord.moddingcommunity.com) if you have questions or want help with anything modding related!
