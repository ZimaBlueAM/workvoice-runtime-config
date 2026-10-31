# Sayit keyboard lexicon update

This directory is the auditable hot-update boundary for the custom Chinese keyboard.

- `sayit-keyboard-lexicon.tsv` is generated from the checked-in compact Rime Ice
  export by `Scripts/build_keyboard_lexicon.py`.
- `manifest.json` pins the payload URL, dotted version, entry count, SHA-256 and
  the safe default ranking policy. The runtime config can change
  `rankingMode` (`weightDescending`, `sourceOrder`, or `alphabetical`) and
  `maxCandidatesPerReading` without downloading the payload again.
- The keyboard accepts only HTTPS manifests, validates the payload digest and
  bounded TSV schema, rejects older versions, then atomically replaces the
  App Group copy. A failed update leaves the bundled Rime/Luna dictionaries in
  place.
- Rime Ice and Luna Pinyin license/attribution files remain next to the
  bundled resources in `KeyboardExtension/Resources/`.

To publish a new resource revision, update the upstream resource with its
license evidence, run:

```bash
python3 Scripts/build_keyboard_lexicon.py --version 2026.10.29.2
python3 Scripts/verify_keyboard_lexicon.py
```

Then publish this directory under
`ZimaBlueAM/workvoice-runtime-config/keyboard-lexicon/` and keep both raw URLs
public. Bump the runtime config's `keyboard.lexicon.manifestURL` only after the
raw manifest and payload are available. `Scripts/publish_runtime_config.sh`
copies and verifies this directory together with the runtime JSON. The verifier
must pass before shipping.
