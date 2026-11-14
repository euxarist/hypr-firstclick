# hypr-firstclick

A Hyprland plugin that makes the first click on an unfocused window only focus it, the way macOS behaves. The click is not delivered to the window, so it cannot press a button, follow a link, or place a text cursor by accident. The matching mouse release is swallowed as well, so applications never see a release without a press.

Builds against Hyprland 0.56 through current `main`. Hyprland has no setting for this behaviour, so it is implemented with the cancellable mouse button event on Hyprland's event bus.

## Behaviour

- Left, right, and middle clicks on an unfocused window focus it and go no further. The next click works normally.
- Clicks with any modifier held pass through untouched, so Super+drag window moves, Ctrl+click, and mouse keybinds still work on the first press.
- Clicks on the bar, wallpaper, and other layer surfaces pass through. Only clicks that land on an unfocused window are affected.
- Set `enabled = false` in the config below to turn the behaviour off without unloading the plugin.

## Config

Add to `hyprland.conf`:

```ini
exec-once = hyprctl plugin load $HOME/.local/lib/hyprland-plugins/firstclick.so

plugin {
    firstclick {
        enabled = true
    }
}
```

## Build

The plugin must be built against headers that exactly match the running Hyprland, or it refuses to load with a version mismatch. Hyprland `main` uses C++26 library features, so build with GCC 16 or newer. With Hyprland installed from source under `/opt/hyprland`:

```bash
export PKG_CONFIG_PATH=/opt/hyprland/share/pkgconfig:/opt/hyprland/lib/pkgconfig
make install CXX=g++-16 EXTRA_FLAGS=--no-gnu-unique
hyprctl plugin load $HOME/.local/lib/hyprland-plugins/firstclick.so
hyprctl plugin list
```

A Hyprland upgrade breaks the plugin until it is rebuilt against the new headers. That is normal for Hyprland plugins.

## License

Copyright © 2026 euxaristia. Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for the full text.
