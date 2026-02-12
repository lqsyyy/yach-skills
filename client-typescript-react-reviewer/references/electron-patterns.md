# Electron Patterns Reference

## IPC Communication

### 1. Listener Cleanup (Critical)
Always remove IPC listeners to prevent memory leaks and multiple trigger bugs.

```typescript
// ✅ Hook Pattern
useEffect(() => {
  const handler = (event, data) => { /* ... */ };
  ipcRenderer.on('my-event', handler);
  return () => ipcRenderer.removeListener('my-event', handler);
}, []);

// ✅ Class Pattern
componentDidMount() {
  ipcRenderer.on('my-event', this.handleEvent);
}
componentWillUnmount() {
  ipcRenderer.removeListener('my-event', this.handleEvent);
}
```

### 2. removeAllListeners Caution
Avoid using `removeAllListeners('event')` unless you are sure no other part of the component/app is listening to the same channel. Prefer removing the specific listener function.

## Local Storage

The project uses `electron-store`.

```typescript
// ✅ Preferred: Use business-tools/config-store wrappers if available
import { storage } from 'renderer/_public/business-tools/config-store';
```

## Window Management

Use `getContainer` with caution in Ant Design components when working with multiple windows or overlays.

```typescript
// ✅ Ensure container exists
getContainer: () => document.getElementById('main-container') || document.body
```

## Performance

### Large Data via IPC
Avoid sending massive objects frequently through IPC. Filter or paginate data in the main process if possible.

## Native Integration

### Sensors and Logging
- Use `util.sensorsData.track` for analytics.
- Use `Sentry.captureException` for error tracking in both main and renderer processes.

## Review Checklist

- [ ] Are all `ipcRenderer.on` listeners removed in unmount?
- [ ] Are all `addEventListener` listeners removed?
- [ ] Is `util.locale` used for all UI strings?
- [ ] Are Electron-specific APIs (fs, path) kept out of purely UI components?
