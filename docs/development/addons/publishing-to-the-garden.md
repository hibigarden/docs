# Publishing to the Hibi garden

The [Hibi garden](https://www.hibi.garden/addons/) lists community addons. The same list appears in the **Hibi garden** tab of **Settings → Addons**, where people install them with **Install**. Hibi's maintainers review every listed addon. When they accept it, the garden signs the exact files its entry names, so people can install it from the garden without allowing unsigned addons.

## Start from a template

Use the [addon starter](https://github.com/hibigarden/addon-starter) for an extension; it has an example extension in `addons/hello`. Use the [theme starter](https://github.com/hibigarden/theme-starter) for a theme; it has an example theme in `themes/hello-theme`. Choose **Use this template** on GitHub, then follow the template's readme to rename the package, check it, and install it in Hibi.

Keep each package in its own folder, with its `README.md`, its license in `LICENSE.txt` or `LICENSE.md`, and every compiled file it uses. Hibi refuses files without an extension, such as `LICENSE`, and does not build packages during installation. See [sideloading and sharing](sideloading.md) for the full package rules.

## Choose where Hibi gets your addon

The directory lives in [hibigarden/addons-repository](https://github.com/hibigarden/addons-repository). Each entry in its `catalog.json` uses exactly one of these sources:

- **`path`**: a folder in the directory repository itself, such as `addons/hello`. Choose this when you want the directory to host the package.
- **`repository`**: a public GitHub repository, a full 40-character commit, and optionally the folder that holds `hibi-addon.json`. Choose this to keep publishing from your own repository.
- **`download`**: an HTTPS ZIP URL, its size in bytes, and its lowercase SHA-256 digest. Choose this for release assets that never change.

```json
{
  "id": "hello-theme",
  "name": "Hello theme",
  "description": "A warm light color scheme and a matching dark one.",
  "version": "1.0.0",
  "apiVersion": 2,
  "kind": "theme",
  "authors": [{ "displayName": "Your name" }],
  "license": "MIT",
  "repository": {
    "url": "https://github.com/you/hello-theme",
    "commit": "0123456789abcdef0123456789abcdef01234567",
    "path": "addons/hello-theme"
  }
}
```

Hibi installs exactly the listed commit or ZIP file. A download whose size or digest differs is refused, and so is a package whose ID, version, or kind differs from its entry or whose files differ from its signature. To publish a new version, update the entry in a new pull request. Copy `id`, `name`, `description`, `version`, `apiVersion`, `kind`, and `authors` from `hibi-addon.json` exactly, and state the addon's license as an [SPDX expression](https://spdx.org/licenses/).

Before you submit an entry, you can test a repository entry by installing its commit link, such as `https://github.com/you/hello-theme/tree/<commit>/addons/hello-theme`. See [GitHub links](sideloading.md#github-links).

## Submit it

Follow the directory's [contributing guide](https://github.com/hibigarden/addons-repository/blob/main/CONTRIBUTING.md). Run its checks, install and try your addon in Hibi, then open a pull request that describes what the addon does, which access it asks for, and how you tested it. Reviewers read the package source and manifest. They do not run build scripts or install dependencies from linked repositories. You do not need a signing key or a signature. The directory's checks report that your entry is waiting for its signature.

Merging the pull request accepts the addon. The directory then signs the listed files automatically, adds the signature as `signatures/<id>.json`, and publishes the addon. It appears on [hibi.garden/addons](https://www.hibi.garden/addons/) and in Hibi after **Refresh**, without a new Hibi release. Each new version needs a new pull request and is signed again when it merges.

Once your entry is listed, you can link to it with `hibi://addons/install?id=<id>`. The link opens the Hibi garden in Hibi and asks for the usual install confirmation.

The signature covers only the files your entry lists. People who install your addon another way, such as from your repository URL or a newer commit, get an unsigned package unless you ship a Hibi signature with it. Unsigned packages install only after the user turns on **Allow unsigned addons**. See [signatures and unsigned packages](sideloading.md#signatures-and-unsigned-packages).
