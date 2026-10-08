---
origin:
  - work
---

# Flutter Data Layer (Work)

Service, repository, and model standards required at my company. Part of [[flutter architecture (work)]], covering the first two layers of `Service -> Repository -> Bloc -> View`.

## Services

Thin wrappers around HTTP, platform APIs, or SDK calls.

- Accept required configuration through the constructor, such as `baseUrl`.
- Return raw service responses when possible, such as `http.Response`.
- Do not parse JSON into app models.
- Do not contain UI, navigation, bloc, or business rules.
- Keep headers and endpoint paths close to the request method.

## Repositories

Translate service responses into app data.

- Inject the matching service through the constructor.
- Parse JSON and construct models in the repository.
- Return a `Result<T>` or equivalent project result type.
- Wrap every repository method in `try/catch`. Repositories do not throw for expected API failures.
- Keep API field names in request bodies aligned with backend contracts.
- Provide repositories to the widget tree once at the app root through `RepositoryProvider` / `MultiRepositoryProvider`.

```dart
class UserRepository {
  const UserRepository({
    required UserService userService,
  })
    : _userService = userService;

  final UserService _userService;

  Future<Result<User>> show({
    required String id,
    required String accessToken,
  }) async {
    try {
      final response = await _userService.show(
        userId: id,
        token: accessToken,
      );
      final data = jsonDecode(response.body);

      return Result(
        data: User.fromJson(data),
        statusCode: response.statusCode,
      );
    } catch (_) {
      return Result(
        statusCode: 400,
      );
    }
  }
}
```

## Result

`Result.resultStatus` maps the HTTP status code to a `ResultStatus`:

| Status code | `ResultStatus` |
| --- | --- |
| 200, 201, 204 | `success` |
| 301 | `movedPermanent` |
| 400, 401, 403 | `error` |
| anything else (404, 422, 5xx, ...) | `none` |

Because most failure codes land on `none`, blocs must treat `none` as a failure too. The exhaustive `switch` that enforces this is in [[flutter bloc (work)]]. `RequestStatus`, the enum blocs keep in state, lives in the same file as `Result`.

## Models

Generated immutable models for API data.

- Use `freezed` and `json_serializable` for API models and bloc states. Bloc events use `freezed`.
- Put each model in `lib/models/<model_name>/<model_name>.dart` and export it from `lib/models/models.dart`.
- Use `@JsonKey(name: 'snake_case')` for backend fields. The API uses snake_case, Dart code uses camelCase.
- Add the `part` directives and regenerate after every change:

```bash
dart run build_runner build --delete-conflicting-outputs
```
