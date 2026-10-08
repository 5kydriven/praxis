---
origin:
  - work
---

# Flutter Architecture (Work)

Flutter architecture and coding standards required at my company. For the architecture I use in my own projects, see [[flutter architecture (personal)]]. The packages these standards rely on are listed in [[tools & libraries]].

This note covers the overall shape. The detailed standards live in:

- [[flutter code style (work)]] — naming, formatting, conditionals, null safety, and typed preference keys.
- [[flutter data layer (work)]] — services, repositories, the `Result` type, and models.
- [[flutter bloc (work)]] — bloc rules, widget state versus bloc state, and listeners.
- [[flutter widgets (work)]] — UI standards, widget composition, theming, assets, and localization.
- [[flutter routing (work)]] — route constants and typed route state.

## Core principles

- Prefer simple, readable code over clever abstractions.
- Keep changes scoped to the feature being worked on.
- Make data flow easy to follow: service -> repository -> bloc -> view.
- Split code when it improves ownership or reuse, not just to reduce file size.
- Avoid introducing new packages, patterns, or architecture unless the current project cannot reasonably solve the problem.
- Generated files (`.freezed.dart`, `.g.dart`, localization output) are tool-owned. Regenerate them, never hand-edit them.

## Layers

Direction of dependency:

```text
Service -> Repository -> Bloc -> View
```

Do not skip layers for API-backed features. A new API-backed feature needs a service, a repository exported from `lib/repositories/repositories.dart`, and both wired into the app root.

## Project structure

Shape for medium to large Flutter apps:

```text
lib/
  bloc/                 # App-wide bloc/state/event
  constants/            # Assets, preferences, string keys
  extensions/           # Theme/context/string extensions
  helpers/              # Reusable stateless helper functions and classes
  l10n/                 # ARB source and generated localization files
  models/               # Freezed/json_serializable models
  pages/
    feature_name/
      bloc/
        state/
        bloc.dart
        feature_bloc.dart
        feature_event.dart
      view/
        view.dart
        feature_page.dart
        feature_app_bar.dart
        feature_body.dart
      widgets/
        widgets.dart
        reusable_feature_widget.dart
      feature_name.dart
  repositories/         # API/data repositories
  services/             # HTTP or platform service wrappers
  themes/               # App theme tokens and ThemeData builders
  widgets/              # Truly app-wide reusable widgets
```

- Feature-specific widgets go in `lib/pages/<feature>/widgets/`. App-wide widgets go in `lib/widgets/`.
- Reusable helper functions and classes go in `lib/helpers/`, parallel to `lib/constants/`. Do not place helper files inside unrelated feature folders. Feature-specific logic stays inside its owning feature folder.
- The feature barrel `<feature>.dart` exports `view/view.dart`, `bloc/bloc.dart`, and `widgets/widgets.dart` when it exists.
- Use barrel files only when they reduce import noise and stay obvious.

## Commands

```bash
flutter pub get                                            # install dependencies
flutter gen-l10n                                           # regenerate localizations after editing an ARB file
dart run build_runner build --delete-conflicting-outputs   # regenerate *.freezed.dart / *.g.dart
flutter analyze                                            # lint the whole project
dart analyze lib/pages/<feature>                           # focused check for a small change
dart format <paths>                                        # format touched files
```

## Anti-patterns

Beyond the rules in these notes, avoid:

- Adding abstractions before reuse exists.
- Putting business logic in widgets.
- Introducing a new state management pattern beside Bloc without a strong reason.
- Splitting one screen into many tiny files that make the flow harder to read.
