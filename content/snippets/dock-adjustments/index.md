+++
authors = ["Caleb Trevatt"]
title = "Dock tweaks"
description = "Some quick dock tweaks. Personal preferences."
date = 2024-12-09

[extra]

[taxonomies]
tags = ["Snippet", "Bash", "Shell Scripting", "macOS"]
+++

## Disable Dock Autohide Animation
This tweak wins you back some screen real-estate and makes the dock feel snappier. Following the below makes the dock hidden by default even with non-fullscreened windows. On mouseover, the dock will unhide itself instantly with no animation.

Press <kbd>⌘</kbd> + <kbd>⌥</kbd> + <kbd>D</kbd> to enable autohide.

```bash
defaults write com.apple.dock autohide-time-modifier -float 0
defaults write com.apple.Dock autohide-delay -float 0
killall Dock
```

You can set those `-float` values as desired. 0 is instant.

{% alert(note = true) %}
The dock will still have an activation "pressure" behaviour in fullscreen apps which can't be configured.
Workaround is to fit the window with double click instead of fullscreening. 
{% end %}


To revert, reset those adjustments with `delete`.

```bash
defaults delete com.apple.dock autohide-time-modifier
defaults delete com.apple.Dock autohide-delay
killall Dock
```

