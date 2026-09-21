# RezTank

A community rebuild of the Virindi Tank plugin for Asheron's Call (Decal). It is a
drop-in replacement for `utank2-i.dll`: same file name, same plugin identity, same
dependencies, so Decal, VTClassic, UtilityBelt and VirindiHUDs keep working unchanged.

Current build: **mod.1**

## Install

1. Close Asheron's Call.
2. Open your Virindi Tank folder (the one containing `utank2-i.dll`, usually
   `C:\Games\VirindiPlugins\VirindiTank\`).
3. Rename the existing `utank2-i.dll` to `utank2-i.dll.orig` (your backup).
4. Download `utank2-i.dll` from the [latest release](https://github.com/ExplicitLab/RezTank/releases/latest) into that folder.
5. Start the game and make sure Virindi Tank is enabled in the Decal window.

The VTank window title will read `Virindi Tank v.1.0.0.0 [mod.1]`, and after login chat
shows `Virindi Tank mod.1 (community rebuild) loaded. Profiles: <folder>`.

## Updates

RezTank checks this repository at login. When a newer build is published it downloads it,
verifies it, and installs it for the next game start (the previous build is kept as
`utank2-i.dll.prev`). To receive only the chat notice and install by hand, create an empty
file named `noautoupdate.txt` in the Virindi Tank folder.

Virindi Automatic Updates Filter will not overwrite RezTank with stock Virindi Tank.

## Changes vs. stock Virindi Tank

### mod.1
- Startup no longer crashes when Decal's `ProfilePath` registry value is missing. Profiles
  are read from `ProfilePath` if it is set and the folder exists, otherwise from the folder
  the DLL is in.
- Built-in update channel (announce + self-install) pointing at this repository.
- Plugin exceptions are still logged locally to `Documents\Decal Plugins\uTank2\errors.txt`
  but are no longer uploaded to virindi.net.

## Restore stock

Delete `utank2-i.dll` and rename `utank2-i.dll.orig` back to `utank2-i.dll`. If stock VTank
then fails to start with a NullReferenceException in `PluginCore.Startup`, add a String
value `ProfilePath` = `<your VirindiTank folder>\` under
`HKLM\SOFTWARE\WOW6432Node\Decal\Plugins\{642F1F48-16BE-48BF-B1D4-286652C4533E}`.

## Releasing a new build (maintainer notes)

1. Build the new `utank2-i.dll` (build id bumped, e.g. `mod.2`).
2. Create a GitHub release and attach `utank2-i.dll` as an asset (the asset name must stay
   exactly `utank2-i.dll` so the `releases/latest/download/utank2-i.dll` link keeps working).
3. Edit `latest.txt` on `main`:
   - line 1: the new build id (`mod.2`)
   - line 2: `https://github.com/ExplicitLab/RezTank/releases/latest/download/utank2-i.dll`
   - line 3: SHA-256 of the DLL (optional but recommended)
   - remaining lines: short notes shown in chat
4. Players on older builds see the notice at their next login and the DLL installs itself.

Note: the repository must be **public** for the update check and downloads to work from
inside the game (the plugin fetches without any credentials).
