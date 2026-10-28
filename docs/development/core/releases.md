# Releases and nightly builds

Hibi has two update channels. **Beta (stable)** is the default and receives numbered releases. Betas normally require every check to pass on every platform, but maintainers can explicitly force a beta with failed checks. **Nightly (bleeding edge)** lists every published build, including betas. Back up your notes before installing a nightly.

The updater reads `feed.json` from the rolling `beta` or `nightly` release. Publication generates SHA-512 hashes and sizes from verified installers with `scripts/update-feed.mjs`. The feed pins downloads to an immutable release. That release also contains the applicable `latest.yml`, `latest-linux.yml`, `latest-linux-arm64.yml`, and `latest-mac.yml` metadata for electron-updater.

Before publication, Linux package filenames are normalized to `linux-x64` and `linux-arm64`, replacing `x86_64`, `amd64`, and `aarch64`. Publication fails if two packages would get the same name.

The main process owns channel preferences, feed requests, verified downloads, and installation. Windows NSIS, Linux AppImage (x64 and arm64), and macOS ZIP updates use electron-updater. Debian and Flatpak packages are published as downloads only and never appear in the feed. The macOS feed includes both architectures and pins the ZIP hashes for self-updating builds. Restart installation passes through the normal document flush and unsaved-edit confirmation. Update ordering compares the version first, puts a release above its own nightlies, then orders nightlies by date and numeric workflow run/attempt, never by commit hash.

The **nightly and releases** workflow builds Linux x64, Linux arm64, Windows x64, macOS Apple Silicon, and macOS Intel installers from one fixed commit. Linux arm64 builds and checks run on GitHub's native `ubuntu-24.04-arm` runner. Every release build compiles the app and runs the full required suite on each platform. Download caches can speed up installation; cached builds and previous test results never certify a release.

Required checks run in four isolated shards per platform. After all 20 shards report, the workflow decides the release channel, tag, and version, then packages every platform with that version. Publication rereads the shard reports and verifies that package reports match the source commit, channel, tag, version, and check results. Missing, duplicate, or mismatched check reports stop publication, including forced releases.

| Result | Publication |
| --- | --- |
| All required checks pass, and there are commits since the last beta or an explicit version was requested | Publish a beta release and advance both the beta and nightly listings. |
| A required check fails, or nothing changed and no explicit version was requested, without force | Publish a nightly prerelease and advance only the nightly listing. Failed checks leave the workflow marked as failed. |
| A maintainer explicitly forces the release | Publish a beta and advance both listings. Failed checks remain recorded and produce warnings. |

## Partial releases and automatic updates

Compilation, packaging, or verification failure excludes that platform without blocking the others. A platform contributes installers only when its build, packaging, verification, and final job result succeed and its complete installer set is present: AppImage, Debian, and Flatpak for each Linux architecture; NSIS for Windows; or signed, notarized DMG and ZIP for each macOS architecture. Failed or missing platforms remain visible in the release results. If no platform has verified installers, publication creates a metadata-only release. Release notes, channel listings, and announcements link only to available verified installers and state when none are available.

A beta updates both channel listings; a nightly updates only the nightly listing. Automatic updates advance only when every platform has a complete verified installer set. Partial and metadata-only releases preserve each channel's entire last complete `feed.json`, even for platforms with new installers. The feed uses one tag and version for every platform, and existing macOS readers require both ZIP architectures, so it cannot mix new and older packages. A channel's first partial publication creates a listing without an automatic update feed. Use the versioned release's available downloads for manual installation.

## Betas

Betas use `v<version>` tags and are full GitHub releases, marked as latest when they include installers. By default, each beta increments the patch version of the newest plain `v<semver>` tag. The version in `package.json` is a floor, so raising it starts a new series: with `0.2.0`, the first beta is `v0.2.0`, followed by `v0.2.1`, `v0.2.2`, and so on. To release a minor version, select **Actions → nightly and releases → Run workflow** on `main` and choose **bump: minor**; after `v0.2.9`, that produces `v0.3.0`. Scheduled runs always increment the patch and ignore manual version and force inputs. Pushing a version tag does not start a release.

For an exact version, enter plain semver in **version**, such as `0.3.0`. An existing `v0.3.0` tag is rejected. An explicit version can release an unchanged revision when checks pass. To force a beta despite failed checks, enable **force_release** or append `^` to **version**: `0.3.0^` produces tag `v0.3.0` and package version `0.3.0`. The caret is removed before tagging. Force still runs every check and requires matching source identity and verified, complete installer sets for each published platform. Release notes and announcements disclose the override and retain actual results.

The rolling [beta release](https://github.com/schmayterling/hibi/releases/tag/beta) carries the beta feed. It never moves back to an older commit, so a rerun of an older revision cannot replace a newer beta.

## Nightlies

Nightlies run daily at 18:00 UTC (02:00 Manila time), including when the source commit has not changed. GitHub may delay scheduled runs. You can also select **Actions → nightly and releases → Run workflow** on `main`. The **clean** option bypasses download caches; full builds and checks run either way.

Nightlies use `nightly-<date>-<commit>-<run>-<attempt>` tags and versions such as `0.2.1-nightly.20261025.gabcdef0.12.1`, based on the next beta version. These releases and their assets are not replaced after publication. Release notes list each platform's check and package results and link to the workflow logs. The rolling [nightly release](https://github.com/schmayterling/hibi/releases/tag/nightly) carries the nightly feed.

## Moving from the old nightly channels

Earlier builds followed `nightly-green` (recommended nightly) or `nightly` (every nightly, including broken builds), and can only install old-format nightlies. The last old-format nightly, `0.2.0-nightly.*`, was published to both old feeds and contains the new updater. When it starts, a recommended-nightly install switches to Beta and an every-nightly install switches to Nightly; both then update from the new feeds. The old `nightly-green/update.json`, `nightly-green/recommended-nightly.json`, and `nightly/update.json` files are no longer updated.

## Announcements

Set the repository Actions secret `NIGHTLIES_WEBHOOK_URL` to a Discord webhook URL to announce published betas and nightlies. Each message mentions role `1550309423166267432` and includes the tag, source commit, and available installer links. Download links come from the verified installer checksum manifest and exclude source archives and unavailable platforms. Forced betas disclose the override; metadata-only releases state that no verified installers are available. Failed publications do not send announcements, and a rerun of an already published release does not announce it again. If the secret is unset, announcements are skipped. Delivery failures fail the notification step without undoing publication or changing either feed.

## Publication details

Publication requires no extra token: only the publish job has repository contents write permission. Nightlies remain prereleases and never replace GitHub's latest release. Failed uploads leave a draft for retry from the same source commit. Retrying removes stale draft attachments and replaces only the currently verified files. An already published release keeps its assets and feeds. Temporary Actions artifacts expire after one day.

For local distribution packages, use `npm run dist`. It runs `npm run check` before creating installers. `npm run package` creates an unpacked development app and does not certify a release.

## Addon starter package

The first release requires a maintainer with npm 2FA to run `npm publish --access public` from `packages/create-hibi-addon` after the PR is merged. npm cannot stage a new package or register its trusted publisher before the package exists. Confirm the first release with `npm view create-hibi-addon version` before recommending `npx create-hibi-addon`.

For later versions, configure `create-hibi-addon` on npm with GitHub user `schmayterling`, repository `hibi`, and workflow filename `publish-create-hibi-addon.yml`. Allow `npm stage publish` only. Bump the version in its `package.json`, then run **stage create-hibi-addon** manually on `main`. The workflow tests the generator and stages that version with provenance, but the version is not public yet. Review it with `npm stage list create-hibi-addon` and `npm stage view <stage-id>` or on npmjs.com. A maintainer with 2FA must run `npm stage approve <stage-id>` or approve it on npmjs.com. Confirm the live version with `npm view create-hibi-addon version`. The workflow does not run when a PR merges and needs no npm token.
