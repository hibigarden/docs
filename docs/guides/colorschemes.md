# Colorschemes

Open **Settings → Appearance** and choose **System**, **Light**, or **Dark**. System follows your operating system's appearance. You can choose a different palette for light and dark mode.

Choose **hibsy** (inspired by notesy) for a dark charcoal palette with lavender accents and muted gold highlights.

## App icon on macOS

Under **Settings → Appearance → App icon**, choose the icon independently of Hibi's colors:

- **Follow system** uses the system appearance while Hibi runs. After quitting, macOS chooses the icon using your system icon appearance setting.
- **Follow app appearance** matches Hibi's light or dark appearance.
- **Always light** keeps the light icon.
- **Always dark** keeps the dark icon.

Your choice is saved across restarts. Follow app appearance, Always light, and Always dark apply a custom icon to the installed app, so the last icon remains after quitting. Hibi needs permission to write to its application folder. A reinstall or update can replace the custom icon; Hibi reapplies your saved choice the next time it starts. Development builds change only the running app's icon.

The persistent custom-icon feature is a prototype. It adds Finder metadata to the app bundle. Ordinary signature verification still passes, but strict signature verification rejects that metadata. Choose Follow system to remove Hibi's custom icon. This limitation must be resolved or accepted before release.

Native system icon appearances require macOS 26 or later. Older supported versions use the light bundle icon after quitting in Follow system mode.

## Exported documentation

Choose a built-in palette before [exporting documentation](exporting.md) to keep those colors in the result. Custom addon palettes are not exported.

## Credits and licenses

The [Catppuccin](../licenses/catppuccin.md), [VS Code](../licenses/vscode.md), and [Nord](../licenses/nord.md) palettes are adaptations for Hibi and may differ from the original themes. Their authors have not endorsed these adaptations. Full notices are also available under **Settings → Credits** and in exported sites.
