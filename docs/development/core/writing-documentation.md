# Writing documentation

Reader-facing guides live in `docs/` in the [documentation repository](https://github.com/hibigarden/docs). Use `docs/development/addons/` for addon authors and `docs/development/core/` for app contributors. Hibi keeps internal agent instructions and investigation notes in its own `docs/ai-agents/`, which is excluded from the published site, and third-party license notices in `docs/licenses/`.

Write complete sentences. Explain what someone needs to do, and omit details that do not help them use or develop the feature. If one sentence is enough, stop there. Use short labels for controls and lists for steps or choices. Keep implementation details in the development guides.

## Work on guides with Hibi

Hibi records the documentation revision it bundles in `docs-revision`. `npm run docs` restores that revision into `docs/` and generates the API reference. To edit guides, clone the documentation repository and point Hibi at it with an absolute path:

```sh
HIBI_DOCS_SOURCE=/path/to/docs npm run export:docs
```

While the variable is set, every npm command reads guides from that checkout. Commit guide changes in the documentation repository. To bundle them in Hibi, update `docs-revision` after they merge.

## Update the API reference

The reference generator reads TypeScript declarations and their documentation comments. Update the declaration or its comment in Hibi, then run:

```sh
npm run docs
npm run docs:check
```

Do not edit generated symbol pages directly. Add descriptions beside the relevant declaration, parameter, or member. The generator also writes links from the older reference locations so existing guide links keep working.

## Preview the site

```sh
npm run export:docs
```

Open `out/docs/index.html` in a browser. Check navigation, links, code samples, and narrow layouts. Markdown files under `docs/` are included automatically, except `ai-agents/`.

For the experimental static-folder export with pre-rendered pages and clean URLs, choose a new output directory and pass the published site URL:

```sh
node scripts/export-docs.mjs docs out/docs-site --static --url https://example.com/docs/
```

Serve the output with a static web server. The single-file export remains the default. The exporter refuses to reuse a static output directory so removed pages cannot remain published by accident.

Use relative links between guides, the API reference, and license notices. Link to Hibi source files with `https://github.com/schmayterling/hibi/blob/main/` URLs. Put images within `docs/` and reference them with relative paths.

## Publish

Each pull request in the documentation repository builds the site with Hibi's `main` branch and attaches it as an artifact. Merging to `main` publishes the site. Hibi also starts a rebuild when the viewer, exporter, API reference, or license notices change.
