# Waybar Pomodoro Timer

A simple, lightweight Pomodoro timer for Waybar on Wayland/Hyprland systems.

<img width="525" height="40" alt="image" src="https://github.com/user-attachments/assets/cb2fbd99-6302-4be6-9727-2b5b692fbf1e" />

## Features

- 🍅 Classic 25-minute Pomodoro sessions
- ⏱️ Real-time countdown display in MM:SS format
- 🔔 Desktop notifications on start/stop
- 💾 Persistent timer state (survives waybar restarts)
- 🎨 JSON output with CSS classes for custom styling
- 🪶 Lightweight - pure Python with no external dependencies

## Screenshots

The timer displays in your waybar with three states:
- **Idle**: No timer running
- **Active**: Countdown in progress (e.g., "Remaining: 24:59")
- **Expired**: Timer finished (shows "00:00")

## Requirements

- Python 3.6+
- Waybar
- `notify-send` (libnotify) for notifications
- Wayland/Hyprland compositor

## Installation

### Quick Install

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/waybar-pomodoro.git
cd waybar-pomodoro

# Install scripts to your local bin
mkdir -p ~/.local/bin
cp bin/waybar-pomodoro ~/.local/bin/
cp bin/toggle-pomodoro ~/.local/bin/
chmod +x ~/.local/bin/waybar-pomodoro
chmod +x ~/.local/bin/toggle-pomodoro

# Ensure ~/.local/bin is in your PATH
# Add to ~/.bashrc or ~/.zshrc if needed:
# export PATH="$HOME/.local/bin:$PATH"
```

### Waybar Configuration

Add the Pomodoro module to your `~/.config/waybar/config.jsonc`:

1. **Add to modules list** (in `modules-right`, `modules-left`, or `modules-center`):
   ```jsonc
   "modules-right": [
     "custom/pomodoro",
     // ... other modules
   ]
   ```

2. **Add module configuration**:
   ```jsonc
   "custom/pomodoro": {
     "exec": "~/.local/bin/waybar-pomodoro",
     "return-type": "json",
     "interval": 5,
     "format": "{}",
     "on-click": "~/.local/bin/toggle-pomodoro",
     "tooltip": true
   }
   ```

3. **Restart Waybar**:
   ```bash
   pkill waybar && waybar &
   ```

## Usage

### Starting/Stopping the Timer

Click on the Pomodoro module in your waybar to start or stop the timer.

### Customizing Session Duration

Edit `bin/waybar-pomodoro` and change the `session_time` value (in seconds):

```python
session_time = 1500  # 25 minutes (default)
session_time = 900   # 15 minutes
session_time = 3000  # 50 minutes
```

### Styling with CSS

The module outputs different CSS classes you can style in `~/.config/waybar/style.css`:

```css
#custom-pomodoro.idle {
  color: #888888;
}

#custom-pomodoro.active {
  color: #50fa7b;
  font-weight: bold;
}

#custom-pomodoro.expired {
  color: #ff5555;
  animation: blink 1s infinite;
}

@keyframes blink {
  50% { opacity: 0.5; }
}
```

## Configuration Options

### Waybar Module Options

- `interval`: Update frequency in seconds (default: `5`)
  - Lower = smoother countdown, higher CPU usage
  - Higher = less frequent updates, lower CPU usage
- `format`: Display format (use `"{}"` to show the text from JSON)
- `on-click`: Command to run on click (default: toggle timer)

### Notifications

To disable notifications, edit `bin/toggle-pomodoro` and remove or comment out the `notify-send` lines:

```bash
# notify-send "Pomodoro" "Timer started - 25 minutes" -t 2000
```

## Security & Privacy

- **No data collection**: All data stays local on your machine
- **No network requests**: Works completely offline
- **Minimal permissions**: Only writes to `~/.cache/pomodoro-state.txt`
- **No secrets**: No hardcoded paths or personal information

## Troubleshooting

### Timer doesn't appear in waybar

1. Check if the script is executable:
   ```bash
   ls -l ~/.local/bin/waybar-pomodoro
   ```

2. Test the script manually:
   ```bash
   ~/.local/bin/waybar-pomodoro
   # Should output: {"text": "Idle", "class": "idle", "tooltip": "No timer running"}
   ```

3. Check waybar logs:
   ```bash
   pkill waybar
   waybar -l debug
   ```

### Timer resets when waybar restarts

This shouldn't happen! The timer state is saved to `~/.cache/pomodoro-state.txt`. If it persists:
- Check file permissions on `~/.cache/`
- Verify the state file exists when timer is running: `cat ~/.cache/pomodoro-state.txt`

### Notifications don't appear

Install libnotify:
```bash
# Arch Linux
sudo pacman -S libnotify

# Ubuntu/Debian
sudo apt install libnotify-bin
```

## Development

### Project Structure

```
waybar-pomodoro/
├── bin/
│   ├── waybar-pomodoro    # Main timer script
│   └── toggle-pomodoro    # Toggle helper script
├── docs/
├── README.md
├── LICENSE
└── .gitignore
```

### Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see LICENSE file for details

## Acknowledgments

Built for Waybar on Wayland/Hyprland systems. Inspired by the classic Pomodoro Technique.
