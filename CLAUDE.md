# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a personal tmux configuration repository written in Japanese. It contains tmux configuration files, documentation, and examples for setting up an optimized tmux environment.

## Key Configuration Files

- `configs/.tmux.conf` - Main tmux configuration file
- `configs/.tmux-cheatsheet.txt` - Cheat sheet for key bindings
- `examples/` - Contains basic.conf, advanced.conf, and minimal.conf variants
- `docs/` - Installation guide, customization guide, and keybindings documentation

## Common Commands

### Installation and Setup
```bash
# Install configuration files
cp configs/.tmux.conf ~/.tmux.conf
cp configs/.tmux-cheatsheet.txt ~/.tmux-cheatsheet.txt

# Reload configuration
tmux source-file ~/.tmux.conf

# Kill and restart tmux server
tmux kill-server
```

### Testing Configuration
```bash
# Check tmux version
tmux -V

# Test configuration syntax
tmux source-file ~/.tmux.conf 2>&1 | head -20

# Show current prefix key
tmux show-options -g prefix
```

## Key Configuration Features

### Custom Prefix Key
- Changed from default `Ctrl+b` to `Ctrl+s` for better ergonomics
- Defined in configs/.tmux.conf:2

### Mouse Support
- Full mouse support enabled
- Custom scroll wheel bindings for smooth scrolling
- Shift + wheel for fast scrolling

### Vim-like Navigation
- hjkl keys for pane navigation
- HJKL for pane resizing
- Vim-style key bindings throughout

### Visual Enhancements
- Status bar positioned at top
- Heavy pane borders for better visibility
- Active pane highlighting
- Git branch display in status bar


## File Structure

```
tmux-config/
├── configs/
│   ├── .tmux.conf              # Main configuration
│   └── .tmux-cheatsheet.txt    # Key bindings reference
├── examples/
│   ├── basic.conf              # Basic setup
│   ├── advanced.conf           # Advanced features
│   └── minimal.conf            # Minimal configuration
└── docs/
    ├── installation.md         # Setup instructions
    ├── customization.md        # Customization guide
    └── keybindings.md          # Complete key reference
```

## Common Troubleshooting

### Configuration Not Loading
1. Check tmux version compatibility (requires 3.0+)
2. Verify file permissions (should be 644)
3. Look for syntax errors in configuration
4. Restart tmux server completely

### Mouse Operations Not Working
1. Verify tmux version (mouse support improved in newer versions)
2. Check terminal compatibility
3. Test with different terminal emulators


## Development Notes

- All documentation is in Japanese
- Configuration emphasizes visual clarity and ergonomics
- Supports Linux, macOS, and WSL environments
- Designed for daily development workflows with git integration