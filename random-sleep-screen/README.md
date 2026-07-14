# random-sleep-screen

xovi QMD extensions for randomizing the reMarkable sleep screen, with an on-device folder picker in quick settings.

## Files

- `randomSleepScreen-withSubdirs.qmd` — replaces the stock `randomSleepScreen.qmd`; picks a random PNG from the active folder
- `sleepScreenFolderPicker.qmd` — adds a tappable label to quick settings to cycle the active folder

## Setup

1. Install `ingatellentSettings.qmd` (required for persistence across reboots)
2. Install `randomSleepScreen-withSubdirs.qmd` and `sleepScreenFolderPicker.qmd` via xovi/vellum
3. Create subdirectories under `/home/root/sleepScreens/` and populate with PNGs:

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
- The selection persists across reboots via `ingatellentSettings`

## Dependencies

- [xovi](https://github.com/asivery/xovi)
- [ingatellentSettings.qmd](https://github.com/ingatellent/xovi-qmd-extensions/blob/main/3.27/ingatellentSettings.qmd)
