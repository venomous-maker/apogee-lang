# Changelog

Every release of apogee-lang, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

Needs grav 1.12.0 or newer.

- `LangPackage`: the module connects itself to an apogee-framework app (apogee-package):
  its settings, wiring moved here from the framework.

## 0.1.1 (2026-11-21)

### Performance

- Plural messages are parsed once and kept: `choice()` is about four times as
  fast.
- Placeholders are filled in one pass over the message (no sorting, no
  string per replacement): `get()` with placeholders is twice as fast.
- A lookup takes the lock once instead of up to three times.

## 0.1.0 (2026-11-20)

The first release. Needs grav 1.10.0 or newer.

- `Translator`: JSON message files per locale (groups, nested keys, sentence
  keys), placeholders, plurals with explicit ranges and CLDR plural forms, a
  fallback locale, messages added from code.
- `Locale.negotiate` for `Accept-Language`; the `Lang` facade with `trans` and
  `transChoice`.
