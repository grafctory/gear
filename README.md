# @grafctory/gear

Job executor and shared services for data pipelines.

**@grafctory/gear** runs [@grafctory/flow](https://github.com/grafctory/flow) pipelines as jobs and supplies the shared
services their tasks draw on. A job names the services it needs, and the executor provides them for the duration of the
run. Ready-made tasks come from the libraries built on top of it.

- **Job Executor**: a pipeline run as a job, with every service it uses scoped to that run
- **Shared Services**: the working space, secrets and caches a run needs, built for the run and released as it ends
- **Custom Bindings**: a stubbed, throttled or cached implementation swapped in for a run, leaving the job untouched
- **Platform Bindings**: service implementations for cloud platforms, one package per platform, bound in place of the
	local defaults

> [!IMPORTANT]
>
> Pipelines are server-side workloads targeting [Node.js](https://nodejs.org/) 22 or later, relying on facilities such
> as the filesystem, the process environment and `fetch`. The packages are not intended for the browser.

# Ecosystem

**@grafctory/gear** focuses on job execution and shared services. Data access, content handling and model work belong to
dedicated task libraries. Together they form a complete, modular toolkit for building data pipelines: every library
runs under the same executor and draws on the same services.

| Package                | Description                                                               |
|------------------------|---------------------------------------------------------------------------|
| **@grafctory/gear**     | Job executor and shared services for data pipelines                       |
| [**@grafctory/pipe**][] | Ready-made tasks for retrieving and persisting data from external sources |
| [**@grafctory/mime**][] | Ready-made tasks for parsing and serialising content by media type        |
| [**@grafctory/llms**][] | Ready-made tasks and shared services for AI jobs                          |

[**@grafctory/pipe**]: https://github.com/grafctory/pipe

[**@grafctory/mime**]: https://github.com/grafctory/mime

[**@grafctory/llms**]: https://github.com/grafctory/llms

# Installation

```shell
npm install @grafctory/gear              # job executor and shared services
npm install @grafctory/gear-<platform>   # service package, one per platform
```

> [!WARNING]
>
> TypeScript consumers must use `"moduleResolution": "nodenext"/"node16"/"bundler"` in `tsconfig.json`.
> The legacy `"node"` resolver is not supported.

Install the core package, then add a service package for each platform the job runs on. Service packages are
self-contained leaves, each pulling in only the client libraries its own platform needs.

| Package              | Description                          |
|----------------------|--------------------------------------|
| [@grafctory/gear]     | Job executor and shared services     |
| [@grafctory/gear-gcp] | Google Cloud service implementations |

[@grafctory/gear]: https://grafctory.github.io/gear/modules/_grafctory_gear.html

[@grafctory/gear-gcp]: https://grafctory.github.io/gear/modules/_grafctory_gear-gcp.html

# Usage

> [!NOTE]
>
> Each package documents its own API in its README and API reference; for complete coverage, see the
> [API reference](https://grafctory.github.io/gear/).

A job binds the services it relies on, then drives a [@grafctory/flow](https://github.com/grafctory/flow) feed through the
tasks provided by the ecosystem libraries:

```ts
import { bind, executor, service } from "@grafctory/gear";
import { createDotVault, createVault } from "@grafctory/gear/vault";
import { csv } from "@grafctory/mime-csv";
import { fetch } from "@grafctory/pipe-url";
import { pipe } from "@grafctory/flow";
import { items } from "@grafctory/flow/feeds";
import { each } from "@grafctory/flow/sinks";

await executor(
	bind(createVault, createDotVault)
)(async () => pipe(items([await service(createVault)("data-url")])
	(fetch())
	(csv())
	(each(record => console.log(record)))
));
```

# Support

- open an [issue](https://github.com/grafctory/gear/issues) to report a problem or to suggest a new feature
- start a [discussion](https://github.com/grafctory/gear/discussions) to ask a how-to question or to share an idea

# License

This project is licensed under the Apache 2.0 License –
see [LICENSE](https://github.com/grafctory/gear?tab=Apache-2.0-1-ov-file) file for details.
