# Colorschemes

Open **Settings → Appearance** and choose **System**, **Light**, or **Dark**. System follows your operating system's appearance. You can choose a different palette for light and dark mode.

Choose **hibsy** (inspired by notesy) for a dark charcoal palette with lavender accents and muted gold highlights.

## App icon

Under **Settings → Appearance → App icon**, choose **Follow system**, **Follow app appearance**, **Light**, **Dark**, or **Custom image…**. Follow app appearance is the default and matches the appearance you chose above; Follow system uses your operating system's appearance even when Hibi's differs. Follow system isn't offered on Linux, where Hibi can't read the system appearance while it uses its own; choose Follow app appearance with Hibi's appearance set to System instead.

A custom image can be a PNG, or an ICNS or ICO file with a PNG image inside, on every system, and also an SVG on macOS. SVGs must be self-contained drawings: no scripts, links to other files, or elements outside SVG. Hibi keeps a copy of up to 512 pixels in its app data, so moving or deleting the original does not change the icon. Images larger than 8 MB or 4096 pixels on a side, SVGs larger than 256 KB, and SVGs macOS can't draw are refused. **Reset** removes the copy; if the custom image was the icon in use, the icon goes back to Follow app appearance.

The icon appears in the Dock on macOS, and in the taskbar and window switcher on Windows and Linux, from the moment Hibi opens. While Hibi is closed, a pinned Dock or taskbar icon shows Hibi's standard light icon: on macOS, changing it would mean modifying the installed app, which breaks its signature.

## Exported documentation

Choose a built-in palette before [exporting documentation](exporting.md) to keep those colors in the result. Custom addon palettes are not exported.

## Credits and licenses

The [Catppuccin](../licenses/catppuccin.md), [VS Code](../licenses/vscode.md), and [Nord](../licenses/nord.md) palettes are adaptations for Hibi and may differ from the original themes. Their authors have not endorsed these adaptations. Full notices are also available under **Settings → Credits** and in exported sites.
