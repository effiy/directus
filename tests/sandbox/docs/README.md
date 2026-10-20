---
title: Directus Api Sandbox
tags: [directus, sandbox, testing, docker, cli, api]
category: testing
created: 2021-03-10
updated: 2026-10-20
source: external
type: index
status: stable
lifecycle: active
---

# Directus Api Sandbox

---

## 💡 概述

Utility functions for quickly spinning up and down instances of Directus for usage such as testing or development. The
package offers two ways of interacting: through calling JS functions or through accessing the command line interface.

---

## ⚡ 功能

### CLI

Provides a command-line interface for starting Directus sandbox instances with configurable database backends, watch mode,
schema snapshots, Docker extras, and horizontal scaling.

```
Usage: sandbox [options] <database>

Arguments:
  database                            What database to start the api with (choices: "maria", "cockroachdb", "mssql", "mysql", "oracle", "postgres", "sqlite")

Options:
  -b, --build                         Rebuild directus from source
  -d, --dev                           Start directus in developer mode. Not compatible with build
  -w, --watch                         Restart the api when changes are made
  --inspect                           Start the api with debugger (default: true)
  -p, --port <port>                   Port to start the api on
  -x, --export                        Export the schema to a file every 2 seconds
  -s, --schema [schema]               Load an additional schema snapshot on startup
  --docker.basePort <dockerBasePort>  Minimum port number to use for docker containers
  --docker.keep                       Keep containers running when stopping the sandbox
  --docker.name                       Overwrite the name of the docker project
  -e, --extras <extras>               Enable redis,maildev,saml or other extras
  -i, --instances <instances>         Horizontally scale directus to a given number of instances (default: "1")
  --killPorts                         Forcefully kills all processes that occupy ports that the api would use
  -h, --help                          display help for command
```

### API

The API is accessed through two functions: `sandbox` and `sandboxes`.

```ts
import { sandbox } from '@directus/sandbox';

const sb = await sandbox('postgres', { dev: true });

// Interact via Rest, GQL or WebSockets
const result = await fetch(sb.env.PUBLIC_URL + '/items/articles');

console.log(await result.json());

await sb.close();
```

---

## 📊 指标

「待补充」

---

## 🏛️ 设计

Depending on what is set in the configuration, some of these steps might be skipped:

1. **Building of the API**: If enabled, the API is freshly built each time the sandbox is started. Use the `watch`
   option to quickly iterate on changes.
2. **Starting of the Docker containers**: All required Docker containers like databases or extras like Redis are spun up
   and awaited until healthy. If the Docker containers are still running, they will be reused instead of starting up new
   ones.
3. **Bootstrapping of the database**: If not already bootstrapped, the sandbox will make sure that all necessary
   database tables are created.
4. **Loading of a schema snapshot**: In case the `schema` option is set, the database will also have the snapshot applied
   before starting Directus.
5. **Starting of the API**: Finally, the API(s) are spun up with the right environment variables configured.

---

## 📁 目录

「待补充」

---

## 🔗 资源

- [sandbox](_media/sandbox.md)
- [sandboxes](_media/sandboxes.md)

---

## 🚀 快速开始

「待补充」

---

## ⚙️ 技术栈

- TypeScript
- Node.js
- Docker
- Directus API