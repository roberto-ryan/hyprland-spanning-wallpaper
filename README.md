# Hyprland Spanning Wallpaper

Automatically span a single wallpaper across multiple monitors in Hyprland, accounting for different sizes, positions, and rotations.

## Dependencies

```bash
sudo pacman -S hyprpaper
paru -S rwpspread  # or yay -S rwpspread
```

## Installation

Copy files to your home directory:

```bash
cp -r .config/hypr/* ~/.config/hypr/
cp -r .local/bin/* ~/.local/bin/
chmod +x ~/.local/bin/wallpaper ~/.local/bin/wallpaper-watcher
```

## What's Included

- `.config/hypr/hyprpaper.conf` - Minimal hyprpaper config with IPC enabled
- `.config/hypr/autostart.conf` - Hyprland autostart that runs the wallpaper watcher
- `.local/bin/wallpaper` - Manual wallpaper setter command
- `.local/bin/wallpaper-watcher` - Daemon that auto-spans wallpaper on changes

## Usage

### Automatic
Change wallpapers or themes normally (e.g., via Omarchy) - the watcher detects changes to `~/.config/omarchy/current/background` and auto-spans.

### Manual
```bash
wallpaper /path/to/image.jpg
```

## How It Works

1. `wallpaper-watcher` uses inotify to monitor the wallpaper symlink
2. On change, it calls `rwpspread` which:
   - Reads monitor layout from `hyprctl monitors`
   - Calculates crop regions for each monitor (handling rotation/transforms)
   - Crops the wallpaper into per-monitor sections
   - Applies via hyprpaper IPC

## Notes

- Works with any monitor layout (side-by-side, stacked, mixed orientations)
- For best results, use high-resolution panoramic wallpapers
- Tested on Omarchy Linux (Arch/Hyprland)
