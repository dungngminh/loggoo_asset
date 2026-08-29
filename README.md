# loggoo_asset

Published frame packs. Git is the CMS.

Author a pack on **loggoo_web** `/studio`, export the zip, unzip it here, merge the catalog snippet, commit, push. GitHub Pages serves the files; the app fetches `catalog.json`.

```text
loggoo_asset/
├── catalog.json                 # published-pack index fetched by the app
├── frames/
│   └── <pack-id>/
│       ├── manifest.json        # layout, localized names and pack metadata
│       ├── icon.png             # catalog/studio thumbnail; jpg is also valid
│       ├── story.png            # 9:16 overlay; jpg/jpeg is also valid
│       └── post.png             # 4:5 overlay; jpg/jpeg is also valid
└── <pack-id>.zip                # optional authoring/export artifact
```

`catalog.json` is the publication boundary. Each entry points to one icon and one
manifest under the same `frames/<pack-id>/` directory:

```json
{
  "catalogVersion": 2,
  "packs": [
    {
      "id": "test",
      "schemaVersion": 2,
      "updatedAt": "2026-08-29T14:01:00.000Z",
      "premium": false,
      "names": { "en": "test", "vi": "thử nghiệm" },
      "icon": "frames/test/icon.png",
      "manifest": "frames/test/manifest.json"
    }
  ]
}
```

Increase `catalogVersion` whenever the published pack set or any referenced pack
changes. Keep `id`, `schemaVersion`, `premium`, and `names` aligned between the
catalog entry and its manifest.

Overlays may be png, jpg, or jpeg. Use png when the chrome needs transparent holes so photos show through.

Coordinates in each manifest are fractions of the 360dp frame canvas, not pixels.

Pack schema v2 adds one optional mood face per aspect. Its position is normalized, while `sizeDp`
uses the app's fixed 360dp design canvas:

```json
"mood": {
  "x": 0.72,
  "y": 0.08,
  "sizeDp": 48,
  "rotation": 0
}
```

Omit `mood` when an aspect does not use it. The app renders `FrameDay.topMood` at that placement and
draws nothing when the day has no mood. Schema-v1 packs remain valid and behave as if `mood` were
absent. Catalog entries declare their pack `schemaVersion`; mood geometry stays in `manifest.json`.

Each aspect has the same shape. `slots` and `texts` may contain multiple items;
`mood` is either omitted or contains exactly one placement:

```json
{
  "overlay": "story.jpg",
  "slots": [],
  "texts": [],
  "mood": { "x": 0.72, "y": 0.08, "sizeDp": 48, "rotation": 0 }
}
```

All file references are relative to the pack directory. Do not use absolute paths,
parent-directory segments, or remote URLs in manifests.

Unpublish: delete `frames/<id>/`, drop the entry from `catalog.json`, bump `catalogVersion`, push.
