# Taskbar Left Margin

A [Windhawk](https://windhawk.net/) mod for Windows 11 which shifts the taskbar
content to the right by a configurable amount, leaving an empty margin on the
left. The taskbar background stays full width, and the taskbar context menu
follows the content.

## What it does

- Moves the taskbar's buttons, icons and tray area to the right by a configurable
  number of pixels.
- Keeps the taskbar background full width.
- Moves the taskbar context menu (the jump list) by the same amount, so it stays
  aligned with the taskbar buttons.

## Settings

| Setting | Default | Description |
| --- | --- | --- |
| Left margin (pixels) | 220 | How much empty space to leave on the left of the taskbar content. |
| Follow display DPI | on | Scale the margin with the display DPI, so it keeps the same visual size on high-DPI displays. Turn off to keep it at a constant physical pixel size. |

## Notes

Requires Windows 11.

## Compatibility

- **TranslucentTB** is confirmed compatible and can be used alongside this mod.
- Tested on Windows 11 26H2.

The mod uses XAML diagnostics to modify taskbar elements. There can only be one
XAML diagnostics consumer at a time, so it may conflict with other tools which
use it.

## Suggested use

Together with [FluentFlyout](https://github.com/unchihugo/FluentFlyout): enable
the taskbar widget there, set its position to the bottom left corner, and turn on
the fixed widget width. The taskbar elements then tile linearly instead of
overlapping each other.

## How the context menu is positioned

The taskbar context menu is not laid out by XAML. Its anchor point is computed in
`explorer.exe` by `CTaskListWnd::_ComputeJumpViewPosition` in `taskbar.dll`, and
handed to `ShellExperienceHost.exe`, which draws the menu there. The point is in
physical screen pixels.

Because of that, shifting the taskbar's XAML content does not move the menu — the
menu is not placed relative to it. The mod adjusts the anchor point instead,
which is why it hooks that function. Moving the menu's content, its window or its
popup directly does not work: the content is its own visual tree, and the window
it is drawn into does not correspond to the menu's visible area.

## License

GPL-3.0. The XAML diagnostics and visual tree watching code is based on the
[Windows 11 Taskbar Styler](https://github.com/m417z/my-windhawk-mods) mod by
m417z, which is also licensed under GPL-3.0.

## Feedback

Bug reports and feature requests are welcome in
[Issues](https://github.com/loliri/windhawk-taskbar-left-margin/issues).
