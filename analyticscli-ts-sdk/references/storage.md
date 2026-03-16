# Storage Options

Storage is optional in `@analyticscli/sdk`.

- Without storage: fastest setup, but IDs reset after app restarts.
- With storage: stable `anonId` and `sessionId` across restarts, better continuity for retention and funnels.

Credential source reminder:
- Publishable ingest API key and CLI `readonly_token` come from project **API Keys** in [dash.analyticscli.com](https://dash.analyticscli.com).
- Optional for CLI verification: set a default project once with `analyticscli projects select` (arrow-key picker), or pass `--project <project_id>` per command.

## Adapter Options

| Strategy | Good for | Tradeoff |
| --- | --- | --- |
| No adapter | Fast prototypes and short sessions | IDs reset across restarts |
| `localStorage` | Browser host apps with no extra storage package | Browser-only API |
| `@react-native-async-storage/async-storage` | Standard React Native persistence | Async hydration happens in background; call `ready()` only when you must block first event |
| `react-native-mmkv` | Fast local key-value storage in RN | Native dependency |
| Custom adapter | Existing secure or encrypted store | You own the wrapper |

## Minimal Example

```ts
import { init } from '@analyticscli/sdk';

const analytics = init('<YOUR_APP_KEY>');
```

## Web localStorage Example

```ts
import { init } from '@analyticscli/sdk';

const analytics = init({
  apiKey: process.env.NEXT_PUBLIC_ANALYTICSCLI_PUBLISHABLE_API_KEY ?? '',
  platform: 'web',
  storage: typeof window !== 'undefined' ? window.localStorage : undefined,
});
```

## AsyncStorage Example

```ts
import AsyncStorage from '@react-native-async-storage/async-storage';
import * as Application from 'expo-application';
import { Platform } from 'react-native';
import { init } from '@analyticscli/sdk';

const analytics = init({
  apiKey: process.env.EXPO_PUBLIC_ANALYTICSCLI_PUBLISHABLE_API_KEY,
  debug: __DEV__,
  platform: Platform.OS,
  appVersion: Application.nativeApplicationVersion,
  storage: AsyncStorage,
});
```

## MMKV Example

```ts
import { MMKV } from 'react-native-mmkv';
import { Platform } from 'react-native';
import { init } from '@analyticscli/sdk';

const kv = new MMKV();

const analytics = init({
  apiKey: process.env.EXPO_PUBLIC_ANALYTICSCLI_PUBLISHABLE_API_KEY,
  debug: __DEV__,
  platform: Platform.OS,
  storage: {
    getItem: (key) => kv.getString(key) ?? null,
    setItem: (key, value) => kv.set(key, value),
    removeItem: (key) => kv.delete(key),
  },
});
```

There is no "do not start yet" init flag. Tracking starts on `init(...)`; use
`ready()` (or `initAsync(...)`) only when startup should wait for async storage
hydration.
