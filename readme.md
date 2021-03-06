<p align="center">
  <a href="https://fingerprintjs.com">
    <img src="resources/logo.svg" alt="FingerprintJS" width="300px" />
  </a>
</p>
<p align="center">
  <a href="https://github.com/fingerprintjs/fingerprintjs/actions?workflow=Test">
    <img src="https://github.com/fingerprintjs/fingerprintjs/workflows/Test/badge.svg" alt="Build status">
  </a>
  <a href="https://www.npmjs.com/package/@fingerprintjs/fingerprintjs">
    <img src="https://img.shields.io/npm/dt/fingerprintjs2.svg" alt="Total downloads from NPM">
  </a>
  <a href="https://www.npmjs.com/package/@fingerprintjs/fingerprintjs">
    <img src="https://img.shields.io/npm/v/@fingerprintjs/fingerprintjs.svg" alt="Current NPM version">
  </a>
</p>

Makes a website visitor identifier from a browser fingerprint.
Unlike cookies and local storage, fingerprint stays the same in incognito/private mode and even when browser data is purged.

## Quick start

### Install from CDN

```html
<script>
  function onFingerprintJSLoad(fpAgent) {
    // The FingerprintJS agent is ready. Get a visitor identifier when you'd like to.
    fpAgent.get().then(result => {
      // This is the visitor identifier:
      const visitorId = result.visitorId;
      console.log(visitorId);
    });
  }
</script>
<script
  async src="https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.min.js"
  onload="FingerprintJS.load().then(onFingerprintJSLoad)"
></script>
```

We recommend to upload [the JS script](https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.min.js)
to your server because AdBlock and other browser extensions can block the public script URL.

### Alternatively you can install from NPM to use with Webpack/Rollup/Browserify

```bash
npm i @fingerprintjs/fingerprintjs
# or
yarn add @fingerprintjs/fingerprintjs
```

```js
import FingerprintJS from '@fingerprintjs/fingerprintjs';

(async () => {
  // We recommend to call `load` at application startup.
  const fpAgent = await FingerprintJS.load();

  // The FingerprintJS agent is ready. Get a visitor identifier when you'd like to.
  const result = await fpAgent.get();

  // This is the visitor identifier:
  const visitorId = result.visitorId;
  console.log(visitorId);
})();
```

???? [Full documentation](#open-source-version-reference)

## Upgrade to [Pro version](https://fingerprintjs.com) to get 99.5% identification accuracy

<p align="center">
  <a href="https://fingerprintjs.com">
    <img src="resources/pro_screenshot.png" alt="Pro screenshot" width="697px" />
  </a>
</p>

<table>
  <thead>
    <tr>
      <th></th>
      <!-- The <img>s are to make the table take the full width -->
      <th align="center"><img width="350" height="0"> <p>Open Source version</p></th>
      <th align="center"><img width="350" height="0"> <p>Pro version</p></th>
    </tr>
  </thead>
  <tbody>
    <tr><td>Identification accuracy</td><td align="center">60%</td><td align="center">99.5%</td></tr>
    <tr><td>Bot detection</td><td align="center">???</td><td align="center">???</td></tr>
    <tr><td>Incognito / Private mode detection</td><td align="center">???</td><td align="center">???</td></tr>
    <tr><td>Geolocation</td><td align="center">???</td><td align="center">???</td></tr>
    <tr><td>Security</td><td align="center">???</td><td align="center">???</td></tr>
    <tr><td>Server API</td><td align="center">???</td><td align="center">???</td></tr>
    <tr><td>Webhooks</td><td align="center">???</td><td align="center">???</td></tr>
  </tbody>
</table>

Pro result example:

```js
{
  "requestId": "HFMlljrzKEiZmhUNDx7Z",
  "visitorId": "kHqPGWS1Mj18sZFsP8Wl",
  "visitorFound": true,
  "incognito": false,
  "bot": { "probability": 0.96 },
  "browserName": "Chrome",
  "browserVersion": "85.0.4183",
  "os": "Mac OS X",
  "osVersion": "10.15.6",
  "device": "Other",
  "ip": "192.65.67.131",
  "ipLocation": {
    "accuracyRadius": 100,
    "latitude": 37.409657,
    "longitude": -121.965467
    // ...
  }
}
```

???? [Live demo](https://fingerprintjs.com/demo)

??? [How to upgrade from Open Source to Pro in 30 seconds](https://dev.fingerprintjs.com/v3/docs/migrating-from-previous-versions#from-fingerprintjs-open-source-version-3)

???? [FingerprintJS Pro documentation](https://dev.fingerprintjs.com)

## Open-source version reference

### Installation

The library is shipped in various formats:

- Global variable
    ```html
    <script>
      function initFingerprintJS() {
        // ...
      }
    </script>
    <script
      async
      src="https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.min.js"
      onload="initFingerprintJS()"
    ></script>
    ```
- UMD
    ```js
    require(['https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.umd.min.js'], (FingerprintJS) => {/* ... */});
    ```
- ECMAScript module
    ```js
    import FingerprintJS from '@fingerprintjs/fingerprintjs';
    ```
- CommonJS
    ```js
    const FingerprintJS = require('@fingerprintjs/fingerprintjs');
    ```

### API

- `FingerprintJS.load({ delayFallback?: number }): Promise<Agent>`

    Builds an instance of Agent and waits a delay required for a proper operation.
    `delayFallback` is an optional parameter that sets duration (milliseconds) of the fallback for browsers that don't support [requestIdleCallback](https://developer.mozilla.org/en-US/docs/Web/API/Window/requestIdleCallback);
    it has a good default value which we don't recommend to change.

- `agent.get({ debug?: boolean }): Promise<{ visitorId: string, components: {/* ... */} }>`

    Gets the visitor identifier.
    `debug: true` prints debug messages to the console.
    `visitorId` is the visitor identifier.
    `components` is a dictionary of components that have formed the identifier;
    each value is an object like `{ value: any, duration: number }` in case of success
    and `{ error: object, duration: number }` in case of an unexpected error during getting the component.

- `FingerprintJS.hashComponents(components: object): string`

    Converts a dictionary of components (described above) into a short hash string a.k.a. a visitor identifier.
    Designed for extending the library with your own components.

- `FingerprintJS.componentsToDebugString(components: object): string`

    Converts a dictionary of components (described above) into human-friendly format.

## Migrating from v2

- [Migration guide](migrating-v2-v3.md)
- [V2 documentation](https://github.com/fingerprintjs/fingerprintjs/tree/v2)

## Version policy

The library doesn't guarantee the same visitor identifier between versions,
but will try to keep them the same as much as possible.

The documented JS API follows [Semantic Versioning](https://semver.org).
Use undocumented features at your own risk.

## Browser support

```bash
npx browserslist "> 1% in us"
```

## Contribution

See the [contribution guidelines](contributing.md) to learn how to start a playground, test and build.
