# h3-typebox

[![npm version][npm-version-src]][npm-version-href]
[![npm downloads][npm-downloads-src]][npm-downloads-href]
[![Github Actions][github-actions-src]][github-actions-href]
[![Codecov][codecov-src]][codecov-href]

> JSON schema validation for [h3](https://github.com/unjs/h3), using [typebox](https://github.com/sinclairzx81/typebox) & [ajv](https://github.com/ajv-validator/ajv).

## Install

```sh
# Using npm
npm install h3-typebox

# Using yarn
yarn install h3-typebox

# Using pnpm
pnpm install h3-typebox
```

## Usage

```js
import { createServer } from 'http'
import { createApp } from 'h3'
import { validateBody, validateQuery, Type } from 'h3-typebox'

const app = createApp()
app.use('/', async (event) => {
  // Validate body
  const body = await validateBody(event, Type.Object({
    optional: Type.Optional(Type.String()),
    required: Type.Boolean(),
  }))

  // Validate query
  const query = validateQuery(event, Type.Object({
    required: Type.String(),
  }))
})

createServer(app).listen(process.env.PORT || 3000)
```

See how to define your schema with `Type` on [TypeBox documentation](https://github.com/sinclairzx81/typebox#usage).

### Options

You can define a options object on `validateBody` or `validateQuery`. Currently the following options are supported:

- `includeAjvFormats: Boolean`

#### includeAjvFormats

Some formats like `date`, `date-time` or `email` are specified in the current JSONSchema draft, but not included in ajv by default, but provided by the `ajv-formats` package. If one of these formats is needed, you can specify `includeAjvFormats: true` in the options of `validateBody` or `validateQuery` like this:

```ts
// Body
validateBody(event, schema, { includeAjvFormats: true })

// Query
validateQuery(event, schema, { includeAjvFormats: true })
```

Currently, only the following extended formats are supported for performance and security reasons:

- date-time
- time
- date
- email
- uri
- uri-reference

These can be used by custom schemas with the `Type.Unsafe` method or with an inline schema:

```ts
const bodySchema = Type.Object({
  optional: Type.Optional(Type.String()),
  dateTime: Type.String({ format: 'date-time' })
})
validateBody(event, bodySchema, { includeAjvFormats: true })
```

## Development 💻

- Clone this repository
- Install dependencies using `pnpm install`
- Run interactive tests using `pnpm dev`

## License

Made with 💙

Published under [MIT License](./LICENSE).

<!-- Badges -->
[npm-version-src]: https://img.shields.io/npm/v/h3-typebox?style=flat-square
[npm-version-href]: https://npmjs.com/package/h3-typebox

[npm-downloads-src]: https://img.shields.io/npm/dm/h3-typebox?style=flat-square
[npm-downloads-href]: https://npmjs.com/package/h3-typebox

[github-actions-src]: https://img.shields.io/github/workflow/status/kevinmarrec/h3-typebox/CI
[github-actions-href]: https://github.com/kevinmarrec/h3-typebox/actions?query=workflow%3Aci

[codecov-src]: https://img.shields.io/codecov/c/gh/kevinmarrec/h3-typebox/main?style=flat-square
[codecov-href]: https://codecov.io/gh/kevinmarrec/h3-typebox
