Foxdriver
=========

> Foxdriver is a Node library which provides a high-level API to control Firefox over the Remote Debugging Protocol.

## Getting Started

### Installation

To use Foxdriver in your project, run:

```
yarn add foxdriver
# or
npm i foxdriver
```

### Usage

The Firefox Remote Debugging Protocol consists of multiple actors that provide different methods. The Foxdriver API allows you to launch a Firefox instance and connects to the protocol interface automatically. From there you can access the methods of all actors.

__Example__ - opening page and get console.logs

```js
import Foxdriver from 'foxdriver'

(async () => {
    const { client, tab } = await Foxdriver.launch({
        url: 'https://www.mozilla.org/en-US'
    })

    // enable actor
    await tab.console.startListeners()
    // wait until page is loaded
    await new Promise((resolve) => setTimeout(resolve, 3000))
    // receive logs and page errors
    const logs = await tab.console.getCachedMessages()
    console.log(logs)

    // close browser
    client.close()
})()
```

You can also attach yourself to an already running Firefox browser. This requires to start the browser with the `-start-debugger-server=<port>` cli argument and have the following settings set:

- `devtools.chrome.enabled: true`
- `devtools.debugger.prompt-connection: false`
- `devtools.debugger.remote-enabled: true`

To attach yourself to the browser you then need to create a Foxdriver instance with the correct port and host and call the `connect()` method:

```js
import Foxdriver from 'foxdriver'

(async () => {
    const client = new Foxdriver('localhost', 9222)
    await client.connect()

    const tabs = await client.getTabs()

    // ...
})()
```

## API

TBD
