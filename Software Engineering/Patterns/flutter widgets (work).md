---
origin:
  - work
---

# Flutter Widgets (Work)

UI and widget standards required at my company. Part of [[flutter architecture (work)]]. Which state belongs in a widget versus a bloc, and how bloc listeners are written, is covered in [[flutter bloc (work)]].

## Preserve existing design during patches

When a UI patch is requested without a screenshot, Figma reference, or other explicit design specification, the current implementation is the source of truth.

- Limit the patch to the requested behavior and preserve the existing layout, styling, item visibility, and interaction states.
- Do not infer a redesign, remove existing states, or hide existing items.
- Ask before making a visual change that is not represented by the current implementation or explicitly described in the request.
- When a design reference is supplied, apply its explicit changes and use the current implementation for unspecified details.

## Keep composition direct

Do not over-engineer UI into many tiny classes or helper builders. Prefer direct layout inside the screen or section widget when the layout is unique to that place.

```dart
Column(
  crossAxisAlignment: CrossAxisAlignment.start,
  children: [
    Padding(
      padding: const EdgeInsets.only(bottom: 8,),
      child: Text(title),
    ),
    const Padding(
      padding: EdgeInsets.only(bottom: 12,),
      child: Divider(height: 1,),
    ),
    ...items.map(
      (item) => Padding(
        padding: const EdgeInsets.only(bottom: 10,),
        child: FeatureTile(
          icon: Image.asset(item['asset'] as String),
          title: item['title'] as String,
          subtitle: item['subtitle'] as String,
        ),
      ),
    ),
  ],
)
```

## No widget-returning helpers

Do not create helper functions or methods whose purpose is to return a `Widget` or `List<Widget>`, such as `_buildHeader()` or `_buildSection(...)`. Flutter-required callbacks such as `builder:` are allowed.

- If the widget tree is used once, put it directly where it is used.
- If the same tree is used more than once and is large enough that duplication hurts readability, extract it into its own widget file under `lib/pages/<feature>/widgets/`.
- If the repeated UI is small, keep the layout direct rather than adding a helper abstraction.

## Reusable widgets

Create a reusable widget only when the same structure appears in multiple places or when extracting it makes the parent easier to understand.

Good candidates:

- A tile used in multiple lists.
- A repeated app bar pattern.
- A repeated dialog shell.
- A repeated form field row.

Avoid reusable widgets for:

- One-off page sections.
- Temporary UI experiments.
- Simple wrappers around one `Text`, `Row`, or `Column`.
- Data classes that exist only to feed temporary UI.

Extracted widgets take their data and callbacks as constructor parameters instead of reading a bloc directly. For repeated list rows, a simple tile is enough:

```dart
class FeatureTile extends StatelessWidget {
  const FeatureTile({
    required this.icon,
    required this.title,
    required this.subtitle,
    this.trailing,
    super.key,
  });

  final Widget icon;
  final String title;
  final String subtitle;
  final Widget? trailing;
}
```

## One widget per file

One public `StatelessWidget` or `StatefulWidget` per file, and one class per file under `view/`.

Allowed in the same file:

- The private `State` class of a `StatefulWidget`.
- Small private helper methods, only when they contain real logic.

A reusable widget goes in the feature `widgets/` folder as its own file, exported from `widgets.dart`.

## Parent chooses variants

When a parent already has the state needed to choose a view, choose there. Do not hide variant selection inside a child unless the child owns the state.

```dart
Builder(
  builder: (context) {
    if (state.billingCycle == BillingCycle.yearly) {
      return YearlyRewardsSection(
        isClaimed: isClaimed,
      );
    }

    return MonthlyRewardsSection(
      isClaimed: isClaimed,
    );
  },
)
```

## Composition rules

- Never use arrow functions (`=>`) for `builder:` callbacks. Always use a block body with an explicit `return`, even when the callback returns a single widget.
- `BlocBuilder` / `BlocSelector` wraps the whole widget's root, not a nested subtree. It is the parent of the top-level `Padding` / `Container`, not nested one level in.
- Use `Padding` instead of `SizedBox` for spacing unless a `SizedBox` is genuinely required by the layout. Explicit sizing and `SizedBox.shrink()` for conditional UI remain valid uses.
- For spacing between list items, use a separator-based layout such as `ListView.separated` instead of inserting standalone spacing widgets among the items.
- Never use collection `if` or `else` entries inside the children list of a `Row`, `Column`, `Stack`, or other list-based widget. Use explicit widgets such as `Visibility`, `Offstage`, or `SizedBox.shrink()` for conditional UI. Keep `Positioned` as the direct child of a `Stack` when positioning conditional content.
- No `for` loops inside a widget's `build` method. Build the list with `.map(...).toList()` and extract the repeated item into its own widget class instead of inlining it.
- Keep a dedicated widget's decoration and build logic in its widget file. Do not extract decoration into a separate private function unless multiple widgets reuse it.
- No top-level or private file-level variable in a view file when it is used only once. Inline the value at its call site.

## Theming, assets, and localization

- Widgets read colors through `Theme.of(context).colorScheme` and typography through `Theme.of(context).textTheme`.
- Put asset paths in `lib/constants/assets.dart`. Do not inline them in the UI.
- Add new asset folders to `pubspec.yaml` if needed. Prefer the existing folder conventions: `assets/`, `assets/icons/`, `assets/images/`.
- Do not hardcode user-visible text in production UI. Text comes from the ARB files in `lib/l10n/` through the generated `AppLocalizations` (`AppLocalizations.of(context)?.someKey`).

## Open questions

Rules in the source standards that appear to conflict. Not resolved yet.

- List spacing: the rule says to use a separator-based layout such as `ListView.separated`, but the "keep composition direct" example spaces items with a bottom `Padding` inside `.map(...)`.
- Repeated items: one rule says to extract the repeated item into its own widget class, another says small repeated UI should stay direct.
