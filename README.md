# apogee-lang

Translations for [Apogee](https://github.com/venomous-maker) apps: messages
kept per language in JSON files, picked by key, with placeholders, plurals
(each language's own forms), a fallback language, and choosing a language
from a request's `Accept-Language`.

```
lang/en/auth.json    {"failed": "These credentials do not match.", "welcome": "Welcome, :name!"}
lang/es/auth.json    {"welcome": "¡Bienvenido, :name!"}
lang/en/cart.json    {"items": "{0} Your cart is empty|{1} One item|[2,*] :count items"}
lang/es.json         {"Save changes": "Guardar cambios"}
```

```typescript
import "std"
import "apogee-lang"
use apogee.*

let t = new Translator("lang", "en", "en")          // directory, locale, fallback
t.get("auth.welcome", {"name": "Ada"})              // "Welcome, Ada!"
t.choice("cart.items", 3)                           // "3 items"
t.get("Save changes", null, "es")                   // "Guardar cambios"

Lang.use(t)
trans("auth.failed")
Lang.setLocale(Locale.negotiate(req.header("Accept-Language"), t.locales(), "en"))
```

## In an apogee-framework app

Importing the module is all it takes: `LangPackage` (apogee-package) connects it when
the app is created. A translator over the app's `lang/` folder is set as `Lang` and bound in the container.

| Setting | Default |
|---|---|
| `lang.path` | `lang`, in the app folder |
| `app.locale`, `app.fallback_locale` | `APP_LOCALE`, `APP_FALLBACK_LOCALE` (`en`) |

Your own configuration wins over these defaults.


### Each request in its own language

The `locale` middleware sets each request's locale from what it asks for,
among `app.locales` (`APP_LOCALES=en,fr,es`): a `{locale}` route parameter, else
`?lang=`, else a `locale` cookie, else the `Accept-Language` header (by its
weights; `es-MX` takes `es`); else `app.locale`. The response says which in
`Content-Language`.

```typescript
router.middleware(["locale"]).group(fn(r: Router) {
    r.get("/{locale}/welcome", fn(req: Request) -> Response { return Response.text(trans("welcome")) })
})
```

A locale belongs to its thread: `Lang.setLocale("fr")` changes it for this
request (or job, or command) only, and other requests keep theirs.
`Lang.withLocale("fr", fn() { ... })` runs some code in another language (a
mail in the recipient's) and puts it back; `Lang.setDefaultLocale` changes the
app's default.

## Keys

`"auth.failed"` is `"failed"` in `lang/<locale>/auth.json`; nested objects
make deeper keys (`"validation.custom.email.required"`). A key that is not
found that way is looked up in `lang/<locale>.json`, where whole sentences are
the keys. A missing key comes from the fallback locale, and if that has none
either, the key itself is the result. Files are read the first time they are
needed and kept.

## Placeholders

`:name` is replaced by the value, `:Name` by it with a capital first letter,
`:NAME` by it in capitals.

## Plurals

`choice(key, count)` picks a variant of `"one|many"` and fills in `:count`.
Variants may say which counts they are for, `{0}`, `{1,2}`, `[2,10]`,
`[11,*]`; otherwise the language's own plural forms decide (Russian has three,
Arabic six, Japanese one, ...: CLDR's rules, as `PluralRules.index(locale, n)`).

## API

| | |
|---|---|
| `new Translator(directory, locale = "en", fallback = "en")` | |
| `get(key, replace = null, locale = "")` | |
| `choice(key, count, replace = null, locale = "")` | |
| `has(key, locale = "")` | in that locale itself (not the fallback) |
| `addLines(locale, group, lines)` | messages from code |
| `locales()` | the locales the directory has |
| `Locale.negotiate(acceptLanguage, available, fallback)` | the best match, a region falling back to its language |
| `Lang.use(t)`, `Lang.get`, `Lang.choice`, `Lang.setLocale`, `trans(...)`, `transChoice(...)` | the app's translator |

## Tests

```bash
grav test tests/lang.grav
grav test tests/package.grav
```
