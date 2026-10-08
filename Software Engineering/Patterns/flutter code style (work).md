---
origin:
  - work
---

# Flutter Code Style (Work)

Naming, formatting, and null-safety rules required at my company. Part of [[flutter architecture (work)]].

## Naming and code style

- Files use `snake_case.dart`. Classes use `PascalCase`.
- Bloc events describe user or lifecycle intent: `LoginSubmitPressed`, `ScreenCreated`, `EmailAddressChanged`.
- Asset constants use descriptive camelCase names based on meaning, not file location alone:

```dart
static const profileRingIcon = 'assets/profile_ring_icon.png';
```

- Use simple, plain variable names that state what the value represents, not clever or overly technical naming.
- No comments. Write self-documenting code instead.
- No nested ternary expressions. Use `if/else` statements.
- Always include a trailing comma after the final named parameter in declarations and the final named argument in calls, even when they fit on one line. Keep `trailing_commas: preserve` in `analysis_options.yaml` so `dart format` does not strip them.

```dart
const EdgeInsets.only(
  top: 20,
  bottom: 8,
)
```

- Order conditionals by likelihood:
  - `if`/`else`: the `if` holds the condition most likely to be true, the `else` the rare case, so the common path resolves without falling through.
  - `if` only: the condition checks the rare case that needs the extra work, and that work stays inside the block. No guard clauses or conditional early returns (`if (value == null) return;`). Use positive conditional execution (`if (value != null) { ... }`).
- Use the typed key classes in `lib/constants/` (`StringPreferences`, `BoolPreferences`, `IntPreferences`, `StringListPreferences`) for `SharedPreferences` keys instead of inline strings.

## Null safety

- Never use the null-assertion operator (`!`) in hand-written code.
- Handle nullable values with explicit guards, pattern matching, null-aware operators, or safe fallback values.
- Prefer `?` null-safe access with a typed `??` fallback:

```dart
AppLocalizations.of(context)?.someKey ?? ''
```

- Do not replace force unwraps with unsafe casts that only hide nullability.
- Do not use `whereType`.

## Open questions

Rules in the source standards that appear to conflict. Not resolved yet.

- Null handling: "explicit guards" are listed as a valid way to handle nullable values, while guard clauses and conditional early returns are not allowed.
