# GregTech: New Horizons Pterodactyl Egg

Use [`Pterodactyl-GTNH-egg.json`](Pterodactyl-GTNH-egg.json) for new installations. It supports stable, beta, release-candidate and daily GTNH server packs with modern Java. Daily builds do not require a GitHub token.

Java 17, 21, 25 and 26 images are included. Choose a Java version within the range shown in the selected server pack's filename. The startup command sets the maximum Java heap to 95% of the container memory limit and automatically confirms Forge/FML update prompts.

## Install

1. Import `Pterodactyl-GTNH-egg.json` into a nest in the Pterodactyl admin panel.
2. Create a server using that egg and select a compatible Java image.
3. Choose the version using the settings below and let the installation finish.
4. Start the server and accept the Minecraft EULA when prompted.

For stable, beta or release-candidate versions, set `GTNH_DAILY=false` and `GTVERSION=latest` or an exact version such as `2.8.4` or `2.9.0-beta-3`. `latest` selects the newest stable release.

For daily builds, set `GTNH_DAILY=true` and `GTNH_DAILY_BUILD=latest` or an available build number. `GTVERSION` is ignored in daily mode.

## Update GTNH

1. Stop the server and create a **full server backup in the panel**.
2. Select the wanted GTNH version and a compatible Java image in the startup settings.
3. Click **Reinstall** in the server settings.
4. Wait for the installation to complete successfully.
5. Refresh the panel page and start the server.

The installer cannot start the server automatically after reinstalling.

## What is preserved?

World folders are left untouched. The installer temporarily copies and restores these files and folders during updates:

- `config/JourneyMapServer`
- `server.properties`
- `ops.json`, `whitelist.json`, `banned-players.json`, `banned-ips.json`
- `serverutilities/server/ranks.txt`
- `serverutilities/server/players.txt`

This temporary copy is **not a full server or world backup**. Create the full backup through the panel before reinstalling.

The modpack's `config` directory (except JourneyMapServer), `libraries`, `mods`, `java9args.txt` and `lwjgl3ify-forgePatches.jar` are replaced. Save custom mods and other custom configuration separately. If the update replaces `serverutilities/serverutilities.cfg`, check that ServerUtilities ranks are enabled again when needed.

If installation is interrupted, the next attempt restores any completed temporary copy before cleaning up. If restoration fails, the copy stays available. An **unmarked backup** makes the installer stop without deleting it: save `.gtnh-install-work/backup` elsewhere, inspect and recover its files to their corresponding server paths, and only then remove `.gtnh-install-work` and reinstall. Use the full panel backup if that temporary copy is incomplete.

## Update the egg itself

Upload the new JSON through the existing egg's **Update Egg** option in the admin panel. Egg updates are manual; this egg has no update URL configured. Updating the egg definition and reinstalling the GTNH server are separate actions.

For servers created using the older egg, check the version settings after updating and set `GTNH_DAILY_BUILD=latest` if it was empty. Reset the server's startup command to the new egg default:

```text
java -Xms128M -XX:MaxRAMPercentage=95.0 -Dfml.readTimeout=180 -Dfml.queryResult=confirm @java9args.txt -jar lwjgl3ify-forgePatches.jar nogui
```

## Legacy eggs

Older eggs are kept unchanged in [`legacy`](legacy/) for reference. Use the new universal egg for new installations.

- [Original GTNH 2.8.X egg](legacy/Pterodactyl-GTNH-2.8.X-egg.json)
- [GTNH 2.8.X with Twist Space Technology](legacy/Pterodactyl-GTNH-2.8.X-twist-space-technology-egg.json)

The universal egg does not install TST.

## More information

- [Pelican version](https://github.com/Loordi/Pelican-panel-eggs/tree/main/GregTech%20new%20horizons)
- [GTNH server setup and update guide](https://wiki.gtnewhorizons.com/wiki/Server_Setup#Server_Update)
- [Official downloads](https://www.gtnewhorizons.com/downloads/)
- [GTNH Discord](https://discord.gg/gtnh)

Future changes to GTNH download locations, archive names or server layout may require an egg update.
