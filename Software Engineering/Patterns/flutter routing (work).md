---
origin:
  - work
---

# Flutter Routing (Work)

Routing standards required at my company. Part of [[flutter architecture (work)]].

## Route definition

- Define the route as a string constant on the page widget, used directly as the router path:

```dart
static const route = '/feature';
```

- Register the route in the app router.
- Pass complex route data through `state.extra` when that is already the local project pattern.
- Keep navigation in views or listeners, not in repositories or blocs.

## Stateful feature routes

Pages backed by bloc state require a typed `initialState`. Route builders read that state directly from `state.extra`. Do not accept raw primitive alternatives, query-parameter state, or silent fallback state values. The bloc receives that state unchanged through `super(initialState)`, see [[flutter bloc (work)]].

```dart
GoRoute(
  path: FeaturePage.route,
  builder: (context, state) {
    return FeaturePage(
      initialState: state.extra as FeatureState,
    );
  },
),
```

## Stateful feature navigation

Every caller of a stateful feature route passes its typed state through `extra`, including when all state fields use defaults.

```dart
context.push(
  FeaturePage.route,
  extra: const FeatureState(),
);
```
