# @grafctory/gear

[![npm](https://img.shields.io/npm/v/@grafctory/gear)](https://www.npmjs.com/package/@grafctory/gear)

Job executor and shared services for [@grafctory/gear](https://github.com/grafctory/gear).

A consumer sets up an executor, binding the services a job relies on to the implementations chosen for the run. The
executor runs the job, whose tasks resolve each service through a locator, naming it by its default factory rather than
importing a concrete implementation.

Binding a different implementation leaves the job unchanged: the same job runs against the live process surroundings,
against stubs, or against any custom service honouring the same contracts.

Bound implementations are constructed as the execution opens, unbound defaults on first use; either way the instance is
shared across the job and, where it implements a disposal protocol, disposed as the job ends, so concurrent or repeated
runs share nothing.

# Installation

```shell
npm install @grafctory/gear
```

> [!IMPORTANT]
>
> Node.js 22 or later is required.

> [!WARNING]
>
> TypeScript consumers must use `"moduleResolution": "nodenext"/"node16"/"bundler"` in `tsconfig.json`.
> The legacy `"node"` resolver is not supported.

# Usage

| Module                        | Description                      |
|-------------------------------|----------------------------------|
| [@grafctory/gear][gear]        | Job executor and service locator |
| [@grafctory/gear/space][space] | Working space access             |
| [@grafctory/gear/vault][vault] | Sensitive parameter lookup       |
| [@grafctory/gear/cache][cache] | Bulk content caching             |

[gear]: https://grafctory.github.io/gear/modules/_grafctory_gear.index.html

[space]: https://grafctory.github.io/gear/modules/_grafctory_gear.space.html

[vault]: https://grafctory.github.io/gear/modules/_grafctory_gear.vault.html

[cache]: https://grafctory.github.io/gear/modules/_grafctory_gear.cache.html

# Support

- open an [issue](https://github.com/grafctory/gear/issues) to report a problem or to suggest a new feature
- start a [discussion](https://github.com/grafctory/gear/discussions) to ask a how-to question or to share an idea

# License

This project is licensed under the Apache 2.0 License –
see [LICENSE](https://github.com/grafctory/gear?tab=Apache-2.0-1-ov-file) file for details.
