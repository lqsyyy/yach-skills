# Redux-Saga Patterns Reference

## Core Effects

### call (Sync/Async calls)
Use `call` for any function call (especially promises) to make them testable and handled by the middleware.

```typescript
// ✅ CORRECT
const result = yield call(api.fetchUser, userId);

// ❌ WRONG
const result = yield api.fetchUser(userId); // Hard to test, bypasses middleware logic
```

### put (Dispatch actions)
Use `put` to dispatch actions to the store.

```typescript
// ✅ CORRECT
yield put({ type: 'FETCH_SUCCESS', payload: data });

// ❌ WRONG
dispatch({ type: 'FETCH_SUCCESS', payload: data }); // Don't use dispatch inside sagas
```

### takeLatest vs takeEvery
- **takeLatest**: Cancels any previous task if a new one is dispatched (Ideal for data fetching).
- **takeEvery**: Allows multiple tasks to run concurrently.

```typescript
// ✅ RECOMMENDED for Fetching
yield takeLatest(ActionTypes.FETCH_DATA, fetchDataSaga);
```

## Error Handling

**Mandatory:** Always wrap effect logic (especially `call` and `select`) in `try-catch`. An unhandled exception in a saga will terminate the generator, causing the feature to stop working until the app is reloaded.

```typescript
function* fetchDataSaga(action) {
  try {
    const response = yield call(api.getData, action.payload);
    yield put(actions.saveData(response));
  } catch (error) {
    yield put(actions.handleError(error));
    // Optional: Log to Sentry
    Sentry.captureException(error);
  }
}
```

## Parallel Execution

Use `all` to run multiple effects in parallel.

```typescript
// ✅ Parallel
const [users, posts] = yield all([
  call(api.fetchUsers),
  call(api.fetchPosts)
]);
```

## Blocking vs Non-blocking

- **call**: Blocking (waits for the function to return).
- **fork**: Non-blocking (starts a task in the background).

```typescript
// ✅ Non-blocking background task
yield fork(trackAnalyticsSaga, 'page_view');
```

## Root Saga Pattern

```typescript
export default function* rootSaga() {
  yield all([
    takeLatest(ActionTypes.USER_LOAD, userSaga),
    takeLatest(ActionTypes.POSTS_LOAD, postsSaga),
  ]);
}
```

## Review Checklist for Sagas

- [ ] Does the saga use `try-catch`?
- [ ] Are all API calls wrapped in `call`?
- [ ] Are actions dispatched via `put`?
- [ ] is `takeLatest` used where race conditions might occur?
- [ ] Are complex flows broken down into smaller sagas?
- [ ] Are there any missing `yield` keywords?
