# Signed add-on files (self-distribution only)

**Only the self-distributed build carries `update_url`.** The AMO validator
rejects the key outright for Mozilla-hosted add-ons, so the zip uploaded to
AMO never contains it. This directory is for the separate build made with
`python3 build.py --self-distributed`, which declares:

    "update_url": "https://merry8989-uk.github.io/adskip/updates.json"

This file is only used when the add-on is distributed **outside** AMO — the
"On your own" option in the Developer Hub. Add-ons distributed by AMO update
themselves and ignore `update_url` completely.

## Publishing a new version

1. Submit the new `.zip` in the Developer Hub and choose **On your own**.
2. When Mozilla has signed it, download the `.xpi` from the add-on's page.
3. Commit that file here as `adskip-firefox-<version>.xpi` (exact name).
4. Add an entry at the top of `../updates.json`:

   ```json
   {
     "version": "<version>",
     "update_link": "https://merry8989-uk.github.io/adskip/xpi/adskip-firefox-<version>.xpi",
     "update_hash": "sha256:<sha256 of the xpi>"
   }
   ```

   Keep older entries below it; Firefox picks the highest version that is
   newer than the installed one.
5. Push. Pages serves the new file within a minute, and installed copies
   update themselves on the next check (Firefox checks roughly daily, or
   immediately from `about:addons` → gear → **Check for updates**).

`update_hash` is optional but worth including: it makes Firefox refuse a
tampered or partially uploaded file.

## Never commit

Unsigned builds. Firefox only installs `.xpi` files signed by Mozilla, and an
unsigned file at the update link makes every update check fail.
