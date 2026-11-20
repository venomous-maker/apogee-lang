# Changelog

Every release of apogee-lang, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

The first release. Needs grav 1.10.0 or newer.

- `Translator`: JSON message files per locale (groups, nested keys, sentence
  keys), placeholders, plurals with explicit ranges and CLDR plural forms, a
  fallback locale, messages added from code.
- `Locale.negotiate` for `Accept-Language`; the `Lang` facade with `trans` and
  `transChoice`.
