# hibi documentation

This repository holds the source of the [Hibi documentation](https://docs.hibi.garden). Guides are Markdown files in `docs/`, and [docs/README.md](docs/README.md) is the user guide home page.

[Hibi](https://github.com/schmayterling/hibi) generates the API reference, keeps the third-party license notices, and builds the site, so those pages are not in this repository.

## Edit the guides

Follow [Writing documentation](docs/development/core/writing-documentation.md). To preview your changes, point a Hibi checkout at this repository with an absolute path:

```sh
cd hibi
npm ci
HIBI_DOCS_SOURCE=/path/to/docs npm run export:docs
```

Then open `hibi/out/docs/index.html` in a browser.

## Publishing

Each pull request builds the site with Hibi's `main` branch and attaches it as the `documentation` artifact. Merging to `main` publishes the site. Hibi also starts a rebuild when its exporter, viewer, API reference, or license notices change. The published `revision.txt` records the documentation and Hibi commits used.

## License

The documentation is licensed under the GNU General Public License, version 3 only. See [LICENSE.md](LICENSE.md) and [LICENSE-MIT](LICENSE-MIT).
