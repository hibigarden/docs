# Create an importer

Importers convert exported files before the core writes them into a workspace. They are currently available to bundled native addons. Installed renderer-only addons cannot register native import handlers.

Add an `importer` field to the manifest with `sources: ['folder', 'zip']` and short `instructions`. The addon appears in the Import dialog while enabled. Its renderer entry can use an empty `start` method if it needs no other UI.

In `native.ts`, provide an asynchronous `import(files)` function. Each `ImportFile` contains a relative `path` and `Uint8Array` `data`. Return `{ files, warnings }`; warnings are optional messages shown after a successful import. Preserve attachments and relative links when changing paths.

```typescript
import type { NativeAddon } from '../api'

export default {
  id: 'my-importer',
  methods: {},
  async import(files) {
    return { files: [...files] }
  },
} satisfies NativeAddon
```

Core owns the file picker, archive validation, destination selection, and file writes. It validates converter output again before creating a new folder. Do not execute imported content or modify the source export. Test path traversal, duplicate names, malformed exports, links, and cancellation as well as successful imports.

## Compiled streaming importers

The bundled Obsidian, Notion, and Bear importers use a private `importStream(stream)` hook alongside the byte-array `import(files)` fallback. This hook is not part of the public SDK. Its `paths` list identifies the source files; `read(index)` loads bytes only when conversion needs them, and `write(entry)` stages output. An entry with `data` contains converted bytes; an entry with `original` names a source index to copy without buffering its attachment bytes. Keep the fallback for the existing native contract.

Run expensive conversion in a worker, read only note bytes, and await each staged write. Observe the stream's optional abort signal before reads, conversion requests, and writes. Stop pending worker work when aborted and release workers and streams in cleanup. Core checks the workspace and addon activation again before publishing and removes private staging on failure or cancellation. Publication is the commit point; cancellation does not undo an import already published.

Folder discovery has no file-count, total-byte, or attachment-size limit. The bundled converters decode notes and CSV files up to 2 MiB each; buffered inputs and converted output files remain bounded at 32 MiB. CSV conversion has no separate row or column limit. ZIP imports retain their [archive safety limits](../../guides/importing.md).

The byte-array `import(files)` signature remains compatible in API versions 1 and 2. Core buffers at most 256 MiB for a legacy importer, with a 32 MiB per-file bound. That fallback still requires in-memory conversion; use the private compiled streaming path for large folder imports.

See [NativeAddon](../addon-api-reference/NativeAddon.md), [ImportFile](../addon-api-reference/ImportFile.md), and the bundled importer addons for working examples.
