# Changelog

Every release of apogee-lang, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

## 0.2.0 (2026-11-25)

Needs grav 1.12.0 or newer.

- Per-request locales: `Lang.setLocale` is per thread (`setDefaultLocale` for
  the app's), `Lang.withLocale`, and the `locale` middleware (route parameter,
  `?lang=`, cookie, Accept-Language; `app.locales`). Needs grav 1.12.0.
- `LangPackage`: the module connects itself to an apogee-framework app (apogee-package):
  its settings, wiring moved here from the framework.

### Dependencies

- Uses apogee-package 0.1.0, apogee-http 0.3.0, apogee-routing 0.3.0 (from their GitHub tags).

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
