# loggoo_asset

Published frame packs. Git is the CMS.

Author a pack on **loggoo_web** `/studio`, export the zip, unzip it here, merge the catalog snippet, commit, push. GitHub Pages serves the files; the app fetches `catalog.json`.

```
catalog.json
frames/<id>/manifest.json
frames/<id>/icon.png   (or .jpg)
frames/<id>/story.png  (or .jpg)
frames/<id>/post.png   (or .jpg)
```

Overlays may be png, jpg, or jpeg. Use png when the chrome needs transparent holes so photos show through.

Coordinates in each manifest are fractions of the 360dp frame canvas, not pixels.

Unpublish: delete `frames/<id>/`, drop the entry from `catalog.json`, bump `catalogVersion`, push.
