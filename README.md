# Hyprland Spanning Wallpaper

Automatically span a single wallpaper across multiple monitors in Hyprland, accounting for different sizes, positions, and rotations.

## Dependencies

```bash
sudo pacman -S hyprpaper inotify-tools zenity imagemagick
paru -S rwpspread  # or yay -S rwpspread
```

## Installation

Copy files to your home directory:

```bash
cp -r .config/hypr/* ~/.config/hypr/
cp -r .local/bin/* ~/.local/bin/
chmod +x ~/.local/bin/wallpaper ~/.local/bin/wallpaper-watcher ~/.local/bin/wallpaper-pick
```

## What's Included

- `.config/hypr/hyprpaper.conf` - Minimal hyprpaper config with IPC enabled
- `.config/hypr/autostart.conf` - Hyprland autostart that runs the wallpaper watcher
- `.local/bin/wallpaper` - Wallpaper setter with fast IPC path for pre-split tiles
- `.local/bin/wallpaper-watcher` - Daemon that auto-spans wallpaper on symlink changes
- `.local/bin/wallpaper-pick` - Interactive GTK file picker for saved wallpapers

## Usage

### Pick a wallpaper interactively
```bash
wallpaper-pick
```
Opens a GTK file chooser with thumbnail previews. Tiles are cached by image hash so repeated picks apply instantly.

### Set a wallpaper manually
```bash
wallpaper /path/to/image.jpg
```

### Hyprland keybindings
```conf
bindd = SUPER ALT, W, Random wallpaper, exec, ~/.local/bin/wallpaper-random
bindd = SUPER ALT SHIFT, W, Previous random wallpaper, exec, ~/.local/bin/wallpaper-random previous
bindd = SUPER CTRL ALT, W, Pick wallpaper, exec, ~/.local/bin/wallpaper-pick
```

### Automatic
Change wallpapers or themes normally (e.g., via Omarchy) - the watcher detects changes to `~/.config/omarchy/current/background` and auto-spans.

## How It Works

1. `wallpaper` sets the wallpaper via hyprpaper IPC using pre-split JPEG tiles (fast path) or falls back to `rwpspread` for inline splitting
2. `wallpaper-watcher` uses inotify to monitor the wallpaper symlink for external changes (theme switches, etc.)
3. A skip-file flag prevents the watcher from redundantly re-applying when `wallpaper` already handled the IPC, avoiding a visible flash
4. `wallpaper-pick` opens a GTK file chooser, caches tiles by content hash, and applies via the fast IPC path

## Notes

- Works with any monitor layout (side-by-side, stacked, mixed orientations)
- For best results, use high-resolution panoramic wallpapers
- Pairs with [omarchy-random-wallpaper](https://github.com/roberto-ryan/omarchy-random-wallpaper) for random wallpapers from Wallhaven
- Tested on Omarchy Linux (Arch/Hyprland)
