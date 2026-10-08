---
origin:
  - work
---

# Flutter Bloc (Work)

Bloc standards required at my company. Part of [[flutter architecture (work)]].

The repositories and the `Result` type that blocs consume are covered in [[flutter data layer (work)]]. How a page receives its `initialState` is covered in [[flutter routing (work)]]. Where `BlocBuilder` sits in the widget tree is covered in [[flutter widgets (work)]].

## Bloc rules

Blocs own screen state, API calls, validation, and event sequencing.

- Inject repositories through the bloc constructor. The page widget builds its own bloc from its `initialState` and `RepositoryProvider.of<T>(context)`.
- Accept an `initialState` in the constructor and pass it directly to the superclass with `super(initialState)`. Do not transform, normalize, or derive a replacement state inside `super(...)`. Callers construct the intended initial state. Lifecycle events perform later loading or derived-state updates.
- Do not inject, store, or globalize an access token as a bloc constructor argument or instance field. Inside each event handler that makes an authenticated request, read the persisted `User` from `SharedPreferences` and keep its `accessToken` in a local variable, so the request always uses the current session:

```dart
final sharedPreferences = await SharedPreferences.getInstance();
final user = User.fromJson(
  jsonDecode(sharedPreferences.getString(StringPreferences.user) ?? ''),
);
final accessToken = user.accessToken;
```

- Keep events semantic and specific.
- Never read `emit.isDone` in handlers. Let event transformers and lifecycle management handle canceled or completed emitters.
- Use `RequestStatus` for operation progress and outcomes instead of boolean flags such as `isLoading`, `isSuccess`, or `showSuccessNotice`. Derive notice visibility from the operation's status and reset it to `RequestStatus.waiting` when dismissed. Keep independent operations on separate status fields so their listeners do not trigger unrelated behavior. Booleans remain appropriate for independent domain values and UI toggles.
- Do not navigate from blocs. Emit state and let the view react.
- Avoid mutable bloc fields or global counters for detecting stale responses when existing state or persisted data can establish freshness. Prefer a reliable server revision or `updatedAt` comparison when available. Otherwise compare a request-local snapshot with the current saved data before applying the response. Recheck after awaited writes before emitting state. Never let a delayed response overwrite a user update or restore a cleared session.
- When an enum result such as `ResultStatus` determines which `state.copyWith` value is emitted, use an exhaustive `switch` instead of a ternary or an `if/else` around `emit`. Group enum cases that share the same emitted state. Do not add an empty `default`: handling every enum value prevents a request from remaining in progress when a new or uncommon status occurs.

```dart
switch (result.resultStatus) {
  case ResultStatus.success:
    emit(
      state.copyWith(
        requestStatus: RequestStatus.success,
      ),
    );
    break;
  case ResultStatus.movedPermanent:
  case ResultStatus.error:
  case ResultStatus.none:
    emit(
      state.copyWith(
        requestStatus: RequestStatus.failure,
      ),
    );
    break;
}
```

## Widget state vs bloc state

Keep temporary UI-only state in the widget when it does not affect app logic:

- Toggling a temporary claimed/unclaimed display.
- Expanding or collapsing a visual section.
- Tracking a local tab or selected row before it has business meaning.

Use bloc state when:

- The value affects API calls.
- Other widgets or routes need it.
- It must survive widget rebuilds as screen state.
- It is part of a loading, success, failure, or validation flow.

## Bloc listeners

Keep a `BlocListener` or `BlocConsumer` listener inline only when its body is a single expression. Extract every multi-line listener into a named private `void` method on the widget or its `State` class.

```dart
void _requestStatusListener(
  BuildContext context,
  FeatureState state,
) {
  if (state.requestStatus == RequestStatus.success) {
    Navigator.of(context).pop();
  }
}

BlocListener<FeatureBloc, FeatureState>(
  listener: _requestStatusListener,
  child: const FeatureBody(),
)
```

A one-line listener may remain inline:

```dart
listener: (context, state) => _controller.text = state.value,
```
