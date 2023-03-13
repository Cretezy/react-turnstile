<div align="center">
 <h1>React Turnstile</h1>
 <img src="preview.png" width="300" />
 <p><a href="https://developers.cloudflare.com/turnstile/">Cloudflare Turnstile</a> integration for React.</p>
 <a href="https://npm.im/@marsidev/react-turnstile">
  <img src="https://badgen.net/npm/v/@marsidev/react-turnstile?style=flat-square" alt="npm version" />
 </a>
 <a href="https://npm.im/@marsidev/react-turnstile">
  <img src="https://badgen.net/npm/dm/@marsidev/react-turnstile?style=flat-square" alt="npm downloads" />
 </a>
 <a href="https://packagephobia.com/result?p=@marsidev/react-turnstile">
  <img src="https://badgen.net/packagephobia/install/@marsidev/react-turnstile?style=square-flat" alt="install size" />
 </a>
 <a href="https://bundlephobia.com/package/@marsidev/react-turnstile">
  <img src="https://badgen.net/bundlephobia/minzip/@marsidev/react-turnstile?style=square-flat" alt="bundle size" />
 </a>
 <a href="https://github.com/marsidev/react-turnstile/actions/workflows/ci.yml"><img src="https://badgen.net/github/checks/marsidev/react-turnstile/main?style=flat-square" alt="CI status"></a>
 <img src="https://img.shields.io/badge/tested_with-playwright-3ea744.svg?style=flat-square" alt="tested with playwright" />
 <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square" alt="PRs are welcome" />
</div>

# Table of contents

* 1. [Features](#features)
* 2. [Demo](#demo)
* 3. [Install](#install)
* 4. [Usage](#usage)
* 5. [Props](#props)
  * 5.1. [Render options](#render-options)
  * 5.2. [Script options](#script-options)
* 6. [Examples](#examples)
  * 6.1. [Rendering the widget](#rendering-the-widget)
  * 6.2. [Rendering the widget with custom props](#rendering-the-widget-with-custom-props)
  * 6.3. [Managing widget rendering status](#managing-widget-rendering-status)
  * 6.4. [Getting the token after solving the challenge](#getting-the-token-after-solving-the-challenge)
  * 6.5. [Interacting with the widget](#interacting-with-the-widget)
  * 6.6. [Interacting with the widget (using TypeScript)](#interacting-with-the-widget-using-typescript)
  * 6.7. [Validating a token](#validating-a-token)
  * 6.8. [Handling widget expiring](#handling-widget-expiring)
* 7. [Contributing](#contributing)
* 8. [Development](#development)
* 9. [Credits](#credits)
* 10. [License](#license)

## Features

* 💪 smart verification with minimal user interaction
* 🕵️‍♀️ privacy-focused approach
* 💉 automatic script injection
* ⚡️ ssr ready

## Demo

<https://react-turnstile.vercel.app/>

## Install

1. [Follow these steps](https://developers.cloudflare.com/turnstile/get-started/) to obtain a free site key and secret key from Cloudflare.
2. Install `@marsidev/react-turnstile` into your React project.

 ```bash
 npm i @marsidev/react-turnstile
 ```

 Or using any custom package manager:

 ```bash
 pnpm add @marsidev/react-turnstile
 ```

 ```bash
 yarn add @marsidev/react-turnstile
 ```

 ```bash
 bun add @marsidev/react-turnstile
 ```

 ```bash
 ni @marsidev/react-turnstile
 ```

## Usage

The only required prop is the `siteKey`.

```jsx
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  return <Turnstile siteKey='1x00000000000000000000AA' />
}
```

## Props

| **Prop**      | **Type**   | **Description**                                                                                                                                                                                                                                                                                                        | **Required** |
| ------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| siteKey       | `string`   | Your sitekey key, get one from [here](https://developers.cloudflare.com/turnstile/get-started/).                                                                                                                                                                                                                       | ✅            |
| options       | `object`   | Widget render options. More info about this options [below](#render-options).                                                                                                                                                                                                                                          |              |
| scriptOptions | `object`   | You can customize the injected `script` tag with this prop. It allows you to add `async`, `defer`, `nonce` attributes to the script tag. You can also control whether the injected script will be added to the document body or head with `appendTo` attribute. More info about this options [below](#script-options). |              |
| onSuccess     | `function` | Callback that is invoked upon success of the challenge. The callback is passed a token that can be validated.                                                                                                                                                                                                          |              |
| onExpire      | `function` | Callback that is invoked when a challenge expires. Read [the docs](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/#refreshing-a-widget) for more info about handling expired widgets.                                                                                                   |              |
| onError       | `function` | Callback that is invoked when there is a network error.                                                                                                                                                                                                                                                                |              |

### Render options

| **Option**        | **Type**  | **Default**               | **Description**                                                                                                                                                                                                                                                                                                                                                                |
| ----------------- | --------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| action            | `string`  | `undefined`               | A customer value that can be used to differentiate widgets under the same `sitekey` in analytics and which is returned upon validation. This can only contain up to 32 alphanumeric characters including `_` and `-`.                                                                                                                                                          |
| cData             | `string`  | `undefined`               | A customer payload that can be used to attach customer data to the challenge throughout its issuance and which is returned upon validation. This can only contain up to 255 alphanumeric characters including `_` and `-`.                                                                                                                                                     |
| theme             | `string`  | `'auto'`                  | The widget theme. You can choose between `light`, `dark` or `auto`.                                                                                                                                                                                                                                                                                                            |
| language          | `string`  | `'auto'`                  | Language to display, must be either: `auto` (default) to use the language that the visitor has chosen, or an ISO 639-1 two-letter language code (e.g. `en`) or language and country code (e.g. `pt-BR`). The following languages are currently supported: `ar-EG`, `de`, `en`, `es`, `fa`, `fr`, `id`, `it`, `ja`, `ko`, `nl`, `pl`, `pt-BR`, `ru`, `tr`, `zh-CN` and `zh-TW`. |
| tabIndex          | `number`  | `0`                       | The `tabindex` of Turnstile’s iframe for accessibility purposes.                                                                                                                                                                                                                                                                                                               |
| responseField     | `boolean` | `true`                    | A boolean that controls if an input element with the response token is created.                                                                                                                                                                                                                                                                                                |
| responseFieldName | `string`  | `'cf-turnstile-response'` | Name of the input element.                                                                                                                                                                                                                                                                                                                                                     |
| size              | `string`  | `'normal'`                | The widget size. Can take the following values: `'normal'`, `'compact'`, or `'invisible'`. The normal size is 300x65px, the compact size is 130x120px. Use `invisible` if your key type is `invisible`, this option will prevent creating placeholder for the widget.                                                                                                          |
| retry             | `string`  | `'auto'`                  | Controls whether the widget should automatically retry to obtain a token if it did not succeed. The default is `'auto'`, which will retry automatically. This can be set to `'never'` to disable retry upon failure.                                                                                                                                                           |
| retryInterval     | `number`  | `8000`                    | When `retry` is set to `'auto'`, `retryInterval` controls the time between retry attempts in milliseconds. The value must be a positive integer less than `900000`. When `retry` is set to `'never'`, this parameter has no effect.                                                                                                                                            |
| refreshExpired    | `string`  | `auto`                    | Automatically refreshes the token when it expires. Can take `auto`, `manual` or `never`. Read [the docs](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/#refreshing-a-widget) for more info about handling expired widgets.                                                                                                                     |
| execution         | `string`  | `render`                  | Execution controls when to obtain the token of the widget and can be on `render` or on `execute`. Read [the docs](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/#execution-modes) for more info.                                                                                                                                               |
| appearance        | `string`  | `always`                  | Appearance controls when the widget is visible. It can be `always`, `execute`, or `interaction-only`. Read [the docs](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/#appearance-modes) for more info.                                                                                                                                          |

> * All this options are optional.
> * Read [the docs](https://developers.cloudflare.com/turnstile/get-started/client-side-rendering/#configurations) to get more info about this options.
> * The widget is wrapped in a `div`, so you can pass any valid `div` prop such as `className`, `id`, or `style`.

### Script options

| **Option**         | **Type**  | **Default**                 | **Description**                                         |
| ------------------ | --------- | --------------------------- | ------------------------------------------------------- |
| nonce              | `string`  | `undefined`                 | Custom nonce for the injected script.                   |
| defer              | `boolean` | `true`                      | Define if set the injected script as defer.             |
| async              | `boolean` | `true`                      | Define if set the injected script as async.             |
| appendTo           | `string`  | `'head'`                    | Define if inject the script in the head or in the body. |
| id                 | `string`  | `'cf-turnstile-script'`     | Custom ID of the injected script.                       |
| onLoadCallbackName | `string`  | `'onloadTurnstileCallback'` | Custom name of the onload callback.                     |

## Exposed methods

The library exposes some methods within the `Turnstile` component's ref, in order to interact with the widget.

| **Method**      | **Description**                                                                                                                                                                                                                                                                                                                                                             | **Returns** |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| `getResponse()` | Returns the widget’s response token.                                                                                                                                                                                                                                                                                                                                        | string      |
| `reset()`       | Resets the widget. Useful if a given widget has timed out, expired or needs to be reloaded.                                                                                                                                                                                                                                                                                 | void        |
| `remove()`      | Removes the widget. Useful when a widget is no longer needed. This will not call any callback and will remove all related DOM elements.                                                                                                                                                                                                                                     | void        |
| `render()`      | Renders the widget. Since all widgets are rendered automatically, this only takes effect if the widget was previously removed. If the widget is already rendered, this method will not re-render the widget.                                                                                                                                                                | void        |
| `execute()`     | If `options.execution` is set to `'execute'`, this method is used to render the widget. If the widget is already shown (rendered and executed), this method will not re-render the widget. If the widget got removed (`.remove()`), you need to call `.render()` and then `.execute()`. If `options.execution` is set to `'render'` (default), this method takes no effect. | void        |

## Examples

### Rendering the widget

```jsx
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  return <Turnstile siteKey='1x00000000000000000000AA' />
}
```

### Rendering the widget with custom props

```jsx
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  return (
    <Turnstile
      siteKey='1x00000000000000000000AA'
      className='fixed bottom-4 right-4'
      options={{
        action: 'submit-form',
        theme: 'light',
        size: 'compact',
        language: 'fr',
      }}
      scriptOptions={{
        appendTo: 'body'
      }}
    />
  )
}
```

### Managing widget rendering status

```jsx
import { useState } from 'react'
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  const [status, setStatus] = useState()

  return (
    <Turnstile
      siteKey='1x00000000000000000000AA'
      onError={() => setStatus('error')}
      onExpire={() => setStatus('expired')}
      onSuccess={() => setStatus('solved')}
    />
  )
}
```

### Getting the token after solving the challenge

```jsx
import { useState } from 'react'
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  const [token, setToken] = useState()

  return (
    <Turnstile
      siteKey='1x00000000000000000000AA'
      onSuccess={(token) => setToken(token)}
    />
  )
}
```

### Interacting with the widget

```jsx
import { useRef } from 'react'
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  const ref = useRef(null)

  return (
    <>
      <Turnstile ref={ref} siteKey='1x00000000000000000000AA'/>

      <button onClick={() => alert(ref.current?.getResponse())}>
        Get response
      </button>

      <button onClick={() => ref.current?.reset()}>
        Reset widget
      </button>

      <button onClick={() => ref.current?.remove()}>
        Remove widget
      </button>

      <button onClick={() => ref.current?.render()}>
        Render widget
      </button>
    </>
  )
}
```

### Interacting with the widget (using TypeScript)

```jsx
import { useRef } from 'react'
import { Turnstile, type TurnstileInstance } from '@marsidev/react-turnstile'

function Widget() {
  const ref = useRef<TurnstileInstance>(null)

  return (
    <>
      <Turnstile ref={ref} siteKey='1x00000000000000000000AA'/>

      <button onClick={() => alert(ref.current?.getResponse())}>
        Get response
      </button>
    </>
  )
}
```

### Validating a token

```jsx
// LoginForm.jsx
import { useRef } from 'react'
import { Turnstile } from '@marsidev/react-turnstile'

export default function LoginForm() {
  const formRef = useRef(null)

  async function handleSubmit(event) {
    event.preventDefault()
    const formData = new FormData(formRef.current)
    const token = formData.get('cf-turnstile-response')

    const res = await fetch('/api/verify', {
      method: 'POST',
      body: JSON.stringify({ token }),
      headers: {
        'content-type': 'application/json'
      }
    })

    const data = await res.json()
    if (data.success) {
      // the token has been validated
    }
  }

  return (
    <form ref={formRef} onSubmit={handleSubmit}>
      <input type="text" placeholder="username"/>
      <input type="password" placeholder="password"/>
      <Turnstile siteKey='1x00000000000000000000AA'/>
      <button type='submit'>Login</button>
    </form>
  )
}
```

```js
// `pages/api/verify.js`
// this is an example of a next.js api route
// this code runs on the server
const endpoint = 'https://challenges.cloudflare.com/turnstile/v0/siteverify'
const secret = '1x0000000000000000000000000000000AA'

export default async function handler(request, response) {
  const body = `secret=${encodeURIComponent(secret)}&response=${encodeURIComponent(request.body.token)}`

  const res = await fetch(endpoint, {
    method: 'POST',
    body,
    headers: {
      'content-type': 'application/x-www-form-urlencoded'
    }
  })

  const data = await res.json()
  return response.json(data)
}
```

> Check the [demo](https://react-turnstile.vercel.app/) and his [source code](/packages/example) to see a code similar to the above in action.
> Check [the docs](https://developers.cloudflare.com/turnstile/get-started/server-side-validation/) for more info about server side validation.
> As you might noted, there is three ways to get the token response from a solved challenge:
>
> * by catching it from the `onSuccess` callback.
> * by calling the `.getResponse()` method.
> * by reading the widget response input with name `cf-turnstile-response`. This one is not an option if you set `options.fieldResponse` to `false`.

### Handling widget expiring

> By default, you don't need to handle the widget expiring, unless you set `options.refreshExpired` to `'manual'` or `'never'`.

```jsx
import { useRef } from 'react'
import { Turnstile } from '@marsidev/react-turnstile'

function Widget() {
  const ref = useRef(null)

  return (
    <Turnstile
      ref={ref}
      options={{ refreshExpired: 'manual' }}
      siteKey='1x00000000000000000000AA'
      onExpire={() => ref.current?.reset()}
    />
  )
}
```

## Contributing

Any contributions are greatly appreciated. If you have a suggestion that would make this project better, please fork the repo and create a Pull Request. You can also [open an issue](https://github.com/marsidev/react-turnstile/issues/new).

## Development

* [Fork](https://github.com/marsidev/react-turnstile/fork) or clone this repository.
* Install [pnpm](https://pnpm.io/installation).
* Install dependencies with `pnpm install`.
* You can use `pnpm dev` to start the demo page in development mode, which also rebuild the library when file changes are detected in the `src` folder.
* You also can use `pnpm stub`, which run `unbuild --stub`, a [passive watcher](https://github.com/unjs/unbuild#-passive-watcher) to use the library while developing without needing to watch and rebuild. However, this option [can't be used in an esm context](https://github.com/unjs/jiti/issues/32).

## Credits

Inspired by

* [nuxt-turnstile](https://github.com/danielroe/nuxt-turnstile)
* [svelte-turnstile](https://github.com/ghostdevv/svelte-turnstile)
* [react-google-recaptcha-v3](https://github.com/t49tran/react-google-recaptcha-v3)
* [reaptcha](https://github.com/sarneeh/reaptcha)

## License

Published under the [MIT License](./LICENCE).
