# node-nestjs-structure

Node.js framework NestJS project structure
> Started from this issue: [nestjs/nest#2249](https://github.com/nestjs/nest/issues/2249#issuecomment-494734673)

## Configuration

1. Create a `.env` file
    - Rename the [.env.sample](.env.sample) file to `.env` to fix it.
2. Edit env config
    - Edit the file in the [config](src/config) folder.
    - `default`, `development`, `production`, `test`

## Installation

```sh
# 1. node_modules
npm ci
# 1-1. npm < v7 or Node.js <= v14
npm i
# 2. When synchronize database from existing entities
npm run entity:sync
# 2-1. When import entities from an existing database
npm run entity:load
```

If you use multiple databases in `entity:load`, [modify them.](bin/entity.ts#L45)

## Development

```sh
npm run start:dev
# https://docs.nestjs.com/recipes/repl
npm run start:repl
```

Run [http://localhost:3000](http://localhost:3000)

## Test

```sh
npm test # exclude e2e
npm run test:e2e
```

## Production

```sh
npm run build
# define environment variable yourself.
# NODE_ENV=production PORT=8000 NO_COLOR=true node dist/app
node dist/app
# OR
npm start
```

## Folders

```js
+-- bin // Custom tasks
+-- dist // Source build
+-- public // Static Files
+-- src
|   +-- config // Environment Configuration
|   +-- entity // TypeORM Entities
|   +-- auth // Authentication
|   +-- common // Global Nest Module
|   |   +-- constants // Constant value and Enum
|   |   +-- controllers // Nest Controllers
|   |   +-- decorators // Nest Decorators
|   |   +-- dto // DTO (Data Transfer Object) Schema, Validation
|   |   +-- filters // Nest Filters
|   |   +-- guards // Nest Guards
|   |   +-- interceptors // Nest Interceptors
|   |   +-- interfaces // TypeScript Interfaces
|   |   +-- middleware // Nest Middleware
|   |   +-- pipes // Nest Pipes
|   |   +-- providers // Nest Providers
|   |   +-- * // models, repositories, services...
|   +-- shared // Shared Nest Modules
|   +-- gql // GraphQL Structure
|   +-- * // Other Nest Modules, non-global, same as common structure above
+-- test // Jest testing
+-- typings // Modules and global type definitions

// Module structure
// Add folders according to module scale. If it's small, you don't need to add folders.
+-- src/greeter
|   +-- * // folders
|   +-- greeter.constant.ts
|   +-- greeter.controller.ts
|   +-- greeter.service.ts
|   +-- greeter.module.ts
|   +-- greeter.*.ts
|   +-- index.ts
```

## Implements

- See [bootstrap](src/app.ts), [app.module](src/app.module.ts)
  - Database, Module Router, Static Files, Validation, Pino Logger
- [Global Exception Filter](src/common/filters/exceptions.filter.ts)
- [Global Logging Context Middleware](src/common/middleware/logger-context.middleware.ts)
- [Custom Logger](src/config/logger.config.ts) with nestjs-pino
- [Custom Decorators](src/debug) Example at Nest level
- [Configuration](src/config)
- [Authentication](src/auth) - JWT and Session login with Passport
- [Role-based Guard](src/common/guards/roles.guard.ts)
- Controller Routes
  - [Auth Login](src/base/controllers/auth.controller.ts)
  - [Sample](src/sample/controllers/sample.controller.ts) Parameter and [DTO](src/sample/dto/sample.dto.ts)
  - [CRUD API](src/sample/controllers/crud.controller.ts) Sample
- [Database Query](src/sample/providers/database.service.ts) Example
- [Unit Test](src/sample/providers/crud.service.spec.ts)
- [E2E Test](test/e2e)
- [Shared Modules](src/shared) Example
- [GraphQL Structure](src/gql) Example

## Documentation

```sh
# APP, Compodoc
npm run doc #> http://localhost:8080
# API, Swagger - src/swagger.ts
npm run doc:api #> http://localhost:8000/api
```

### File Naming for Class

```ts
export class PascalCaseSuffix {} //= pascal-case.suffix.ts
// Except for suffix, PascalCase to hyphen-case
class FooBarNaming {} //= foo-bar.naming.ts
class FooController {} //= foo.controller.ts
class BarQueryDto {} //= bar-query.dto.ts
```

### Interface Naming

```ts
// https://stackoverflow.com/questions/541912
// https://stackoverflow.com/questions/2814805
interface User {}
interface CustomeUser extends User {}
interface ThirdCustomeUser extends CustomeUser {}
```

### Index Exporting

```diff
# It is recommended to place index.ts in each folder and export.
# Unless it's a special case, it is import from a folder instead of directly from a file.
- import { FooController } from './controllers/foo.controller';
- import { BarController } from './controllers/bar.controller';
+ import { FooController, BarController } from './controllers';
# My preferred method is to place only one fileOrFolder name at the end of the path.
- import { UtilService } from '../common/providers/util.service';
+ import { UtilService } from '../common';
```

#### Circular dependency

<https://docs.nestjs.com/fundamentals/circular-dependency>

```diff
# Do not use a path that ends with a dot.
- import { FooService } from '.';
- import { BarService } from '..';
+ import { FooService } from './foo.service';
+ import { BarService } from '../providers';
```

### Variables Naming

> refer to [Naming cheatsheet](https://github.com/kettanaito/naming-cheatsheet)

### Links

- [Better Nodejs Project](https://github.com/CatsMiaow/better-nodejs-project)
- [Nest Sample](https://github.com/nestjs/nest/tree/master/sample)
- [Awesome Nest](https://github.com/juliandavidmr/awesome-nestjs)
- [NestJS](https://docs.nestjs.com)
- [TypeORM](https://typeorm.io)
