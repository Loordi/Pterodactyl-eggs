# GregTech: New Horizons Pterodactyl Egg

Use [`Pterodactyl-GTNH-egg.json`](Pterodactyl-GTNH-egg.json) for new installations. It supports stable, beta, release-candidate and daily GTNH server packs with modern Java.

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

Upload the new JSON through the existing egg's **Update Egg** option in the admin panel. Egg updates are manual.

For servers created using the older egg, check the version settings after updating and set `GTNH_DAILY_BUILD=latest` if it was empty. Reset the server's startup command to the new egg default:

```text
java -Xms128M -XX:MaxRAMPercentage=95.0 -Dfml.readTimeout=180 -Dfml.queryResult=confirm @java9args.txt -jar lwjgl3ify-forgePatches.jar nogui
```

## Legacy eggs

Older eggs are kept unchanged in [`legacy`](legacy/) for reference. Use the new universal egg for new installations.

- [Original GTNH 2.8.X egg](legacy/Pterodactyl-GTNH-2.8.X-egg.json)
- [GTNH 2.8.X with Twist Space Technology](legacy/Pterodactyl-GTNH-2.8.X-twist-space-technology-egg.json)

## Twist Space Technology

Import [`Pterodactyl-GTNH-twist-space-technology-egg.json`](Pterodactyl-GTNH-twist-space-technology-egg.json) for the same universal GTNH installer with TST included.

`TST_VERSION` has an empty default and is required. Enter a release tag, such as `0.7.16`, or explicitly enter `latest`. Check the [TST releases](https://github.com/Nxer/Twist-Space-Technology-Mod/releases) and [GTNH compatibility table](https://github.com/Nxer/Twist-Space-Technology-Mod#downloads--requirements) before choosing. Clients must also have a compatible TST version installed.

Every reinstall downloads and validates the selected TST jar before replacing the active pack.

- If the existing TST jar filename identifies the same version, `GregTech.lang`, `config/GregTech.lang` and `config/TwistSpaceTechnology.cfg` are preserved.
- If the version changes, these files are archived under `.gtnh-tst-backups/<timestamp>-<suffix>/` and removed from their active paths, following the upstream TST update instructions. The mods regenerate them when the server starts. Custom TST settings must be reviewed and reapplied to the new configuration manually.
- If no installed TST jar is recognized, any existing settings are preserved and the installer reports this in its log. Conventional `TwistSpaceTechnology-<version>.jar` and `TwistSpaceTechnology <version>.jar` filenames are recognized. Rename an unconventional jar to its correct conventional filename before reinstalling if automatic version-change detection is needed.
- Multiple recognized installed TST jars stop the installation before replacing the pack.

With `latest`, a new TST release can therefore reset settings during an otherwise routine reinstall. Pin a release tag if that is undesirable. The archives are retained across later reinstalls; they are not a substitute for a full server backup.

### Migrating an existing TST server

1. Stop the server and create a full panel backup. Save custom mods and other custom configs separately.
2. Update the existing TST egg through **Update Egg** using the new JSON. Updating a shared egg also changes its definition for other servers using it; import a separate egg for an isolated test.
3. Check the server's existing environment values: keep its current `GTVERSION`, explicitly set `TST_VERSION` to the currently installed jar version, set `GTNH_DAILY=false` and `GTNH_DAILY_BUILD=latest` for an ordinary release. Existing values can survive an egg update, including an old `TST_VERSION=latest`; check them manually. The old `TST_UPDATE` setting is no longer used.
4. Reset the server startup command to the new egg default shown above and choose a compatible Java image. The new command uses a 95% maximum heap, compared with 90% in the older TST egg; a custom command can retain 90% if needed.
5. Reinstall and check the installation log before starting. Migrating with the same detected TST version preserves its settings. Changing the version during migration archives and resets them.

An old installer's leftover unmarked `.gtnh-install-work/backup` intentionally blocks migration. Recover that backup as described above before retrying. The world is left untouched, but modpack configs and custom mods are replaced according to the standard installer behavior.

## More information

- [Pelican version](https://github.com/Loordi/Pelican-panel-eggs/tree/main/GregTech%20new%20horizons)
- [GTNH server setup and update guide](https://wiki.gtnewhorizons.com/wiki/Server_Setup#Server_Update)
- [Official downloads](https://www.gtnewhorizons.com/downloads/)
- [GTNH Discord](https://discord.gg/gtnh)

Future changes to GTNH download locations, archive names or server layout may require an egg update.
