# Browser support

The library supports all popular browsers.
We use the following terminal command to decide which browsers to support:

```bash
npx browserslist "cover 96% in us, not IE < 10, not safari < 9"
```

At the moment, the browsers are:

- **Internet Explorer** 11 (see the section below)
- **Edge** 18 and 85+
- **Chrome** 42+
- **Firefox** 48+
- **Desktop Safari** 11.1+
- **Mobile Safari** 9.3+
- **Samsung Internet** 11.1+
- **Android Browser** 4.1+ (see the section below)

Other browsers will probably also work, but we don't guarantee it.

## Old browsers requirements

### Polyfills

Very old browsers like Internet Explorer 11 and Android Browser 4.1
require a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) polyfill to work.
Add a Promise polyfill before loading the FingerprintJS agent.
Examples for various installation methods:

- Global variable
    ```diff
    + <script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
      <script>
        const fpPromise = new Promise((resolve, reject) => {
          const script = document.createElement('script')
          script.onload = resolve
          script.onerror = reject
          script.async = true
          script.src = 'https://cdn.jsdelivr.net/npm/'
            + '@fingerprintjs/fingerprintjs@3/dist/fp.min.js'
          document.head.appendChild(script)
        })
          .then(() => {
            // ...
          })
      </script>
    ```
    If you use a synchronous loading:
    ```diff
    + <script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.min.js"></script>
      <script>
        // ...
      </script>
    ```
- UMD
    ```diff
      require(
        [
          'https://cdn.jsdelivr.net/npm/@fingerprintjs/fingerprintjs@3/dist/fp.umd.min.js',
    +     'https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js',
        ],
        (FingerprintJS) => {
          // ...
        }
      )
    ```
- ECMAScript module
    ```bash
    # Install the polyfill package first:
    npm i promise-polyfill
    # or
    yarn add promise-polyfill
    ```

    ```diff
    + import 'promise-polyfill/src/polyfill'
      import FingerprintJS from '@fingerprintjs/fingerprintjs'

      // ...
    ```
- CommonJS
    ```bash
    # Install the polyfill package first:
    npm i promise-polyfill
    # or
    yarn add promise-polyfill
    ```

    ```diff
    + require('promise-polyfill/src/polyfill')
      const FingerprintJS = require('@fingerprintjs/fingerprintjs')

      // ...
    ```

### Code syntax

Old browsers like IE11 don't support arrow functions (`=>`). Use the classic function syntax instead:

```diff
  const fpPromise = FingerprintJS.load()

  fpPromise
-   .then(fp => fp.get())
+   .then(function (fp) { return fp.get() })
-   .then(result => {
+   .then(function (result) {
      // Handle the result
    })
```
