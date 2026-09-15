# random-sleep-screen

xovi QMD extensions for randomizing the reMarkable sleep screen, with an on-device folder picker in quick settings.

## Files

- `randomSleepScreen-withSubdirs.qmd` — replaces the stock `randomSleepScreen.qmd`; reads `/home/root/sleepScreens/.chosen` at suspend time
- `sleepScreenFolderPicker.qmd` — adds a tappable label to quick settings to cycle the active folder
- `pre-start` — xovi hook script that watches the selected folder and keeps `/home/root/sleepScreens/.chosen` updated

## Setup

1. Install `ingatellentSettings.qmd` (required for persistence across reboots)
2. Install `randomSleepScreen-withSubdirs.qmd` and `sleepScreenFolderPicker.qmd` via xovi/vellum
3. Copy `pre-start` into `/home/root/xovi/scripts/pre-start/` and make it executable
4. Create subdirectories under `/home/root/sleepScreens/` and populate with PNGs:

```
/home/root/sleepScreens/
    nature/
        forest.png
        mountains.png
    minimal/
        blank.png
        grid.png
```

## Usage

- Open **quick settings** on your reMarkable
- Tap the sleep screen label to cycle: `All` → `nature` → `minimal` → `All` → ...
  - `All` picks randomly from all PNGs directly in `/home/root/sleepScreens/`
  - A named folder picks randomly from PNGs in that subdirectory only
- The picker stores the folder via `ingatellentSettings`. The hook starts a lightweight
  on-device refresher that reads that setting every 2 seconds and writes `.chosen`.
  Folder changes and image file changes are picked up automatically.
- New folders and images do not require a hashtable rebuild. Rebuild only after changing
  the QMD files.

## Deploy

```sh
scp ingatellentSettings.qmd sleepScreenFolderPicker.qmd randomSleepScreen-withSubdirs.qmd root@10.11.99.1:/home/root/xovi/exthome/qt-resource-rebuilder/
scp pre-start root@10.11.99.1:/home/root/xovi/scripts/pre-start/random-sleep-screen.sh
ssh root@10.11.99.1 'chmod +x /home/root/xovi/scripts/pre-start/random-sleep-screen.sh'
```

After copying QMD changes, run **Rebuild Hashtable** in reManager and restart xochitl
or reboot. The hook is started by xovi on startup; to restart it manually:

```sh
ssh root@10.11.99.1 'old="$(cat /tmp/random-sleep-screen-picker.pid 2>/dev/null || true)"; [ -n "$old" ] && kill "$old" 2>/dev/null || true; /home/root/xovi/scripts/pre-start/random-sleep-screen.sh'
```

## Dependencies

- [xovi](https://github.com/asivery/xovi)
- [ingatellentSettings.qmd](https://github.com/ingatellent/xovi-qmd-extensions/blob/main/3.27/ingatellentSettings.qmd)
