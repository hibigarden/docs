# Publishing to the Hibi garden

The [Hibi garden](https://www.hibi.garden/addons/) lists community addons. The same list appears in the **Hibi garden** tab of **Settings → Addons**, where people install them with **Install**. Listing an addon helps people find it, but it does not sign the addon or vouch for it.

## Start from the addon starter

The [addon starter](https://github.com/hibigarden/addon-starter) is a GitHub template with an example extension in `addons/hello` and an example theme in `addons/hello-theme`. Choose **Use this template** on GitHub, then follow its readme to rename a package, check it, and install it in Hibi.

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

Hibi installs exactly the listed commit or ZIP file. A download whose size or digest differs is refused, and so is a package whose ID, version, or kind differs from its entry. To publish a new version, update the entry in a new pull request. Copy `id`, `name`, `description`, `version`, `apiVersion`, `kind`, and `authors` from `hibi-addon.json` exactly, and state the addon's license as an [SPDX expression](https://spdx.org/licenses/).

Before you submit an entry, you can test a repository entry by installing its commit link, such as `https://github.com/you/hello-theme/tree/<commit>/addons/hello-theme`. See [GitHub links](sideloading.md#github-links).

## Submit it

Follow the directory's [contributing guide](https://github.com/hibigarden/addons-repository/blob/main/CONTRIBUTING.md). Run its checks, install and try your addon in Hibi, then open a pull request that describes what the addon does, which access it asks for, and how you tested it. Reviewers read the package source and manifest. They do not run build scripts or install dependencies from linked repositories.

Once your entry is listed, you can link to it with `hibi://addons/install?id=<id>`. The link opens the Hibi garden in Hibi and asks for the usual install confirmation.

Packages without a Hibi signature install only after the user turns on **Allow unsigned addons**. To ask for a signature, see [signatures and unsigned packages](sideloading.md#signatures-and-unsigned-packages).
