# Google Summer of Code Logs

![Header Image](./assets/images/header.jpg)

I, [Yash Kumar Verma](https://www.linkedin.com/in/yash-kumar-verma/) am a third year undergraduate student from India. During the summer of 2021, I worked as a student software developer in Google Summer of Code with [caMicroScope](https://github.com/camicroscope). I am a full stack developer, and used my skills to complete the following objectives.

## Objectives

- [x] Role-Based Access Control System
  - [x] To remove the old attribute-based control system.
  - [x] To implement a role-based access control system.
  - [x] To come up with a new structure for the routes file and migrate old routes to new formats.
  - [x] To cover all routes defined in the application under a control system without explicit declarations.
  - [x] To provision dynamic roles in the system which can be configured/defined as per needs.
  - [x] To load the definitions about the roles in the system via a database in place of static files.
  - [x] To create a responsive and dynamic admin panel in React to configure the rules in real time.
  - [x] To declare APIs that allow the admin to configure the portal in real time.
- [ ] Caching database calls
  - [x] To research a method that works consistently with all functions with minimum changes.
  - [x] To write a robust library (discussed later) to dynamically cache any given function.
  - [x] To future proof the library by extending support for typescript if migration/upgrade happens.
  - [x] Robust testing of the caching library to ensure consistent results.
  - [ ] **Decide which functions are to be cached**
- [x] Objectives completed which were originally not part of the proposal
  - [x] Break the codebase into modules/services that deal with specific functionality.
  - [x] Streamlined use of environment variables in the application.
  - [x] Single point configuration for databases and roles services.
  - [x] Refactor a major part of the main application.
  - [x] Wrote documentation for parts of the project that were not under the scope of the proposal.
  - [x] Established a new code style based on Airbnb's guide.
  - [x] Fixed almost all critical linting issues.
  - [x] Published a standalone library on NPM for caching that can be used in other projects as well.
- Work References
  - [x] [pull request for roles service](https://github.com/camicroscope/Caracal/pull/119)
  - [x] [NPM package for cache - easy-cache](https://github.com/YashKumarVerma/easy-cache)
  - [ ] [YashKumarVerma/camicroscope-admin](https://github.com/YashKumarVerma/camicroscope-admin) to be transferred to camicroscope

---

# Architecture

There has been a fundamental change in the way the application is structured. Even when I was limited on time, I have tried to club together similar functionality in form of modules or services (located in /services). This allows easy testing and a single point of contact for all operations. Earlier there was a lot of redundancy in the codebase, which is now removed as all calls are made to a single service and code is not duplicated.

# Dependency

In order for the cache system to work, a new dependency of redis had to be added. The changes in the deployment will be needed to include a redis container for the cache service.

# Role Bases system

> Services Logging their details at runtime
> ![https://i.imgur.com/WXgHA7D.png](https://i.imgur.com/WXgHA7D.png)

> Role definitons
> ![https://i.imgur.com/naJAKUf.png](https://i.imgur.com/naJAKUf.png)

> Default role definitions : changes done here are hard coded into the application, and are automatically seeded into the database at runtime. This allows deploying the same codebase to multiple clusters with similar configurations, even if all are running on different databases. usecase: different branches of a pathology lab
> ![https://i.imgur.com/oLGcB6x.png](https://i.imgur.com/oLGcB6x.png)
> note the hierchy in the roles that are implemented. Each higher role automatically gets all rights of a lower one.

> Documentation : all services written have extensive documentation that should aid developers working on the project.
> ![https://i.imgur.com/lTBnOUj.png](https://i.imgur.com/lTBnOUj.png)

> Middleware: since the routes are binded to the application during runtime based on the configurations defined in routes.json, a middleware is used to enforce the rights access.
> ![https://i.imgur.com/gFnieXr.png](https://i.imgur.com/gFnieXr.png)

## APIS

- to read roles data
  > GET /api/roles
  > ![https://i.imgur.com/00zSTlP.png](https://i.imgur.com/00zSTlP.png)
- to write roles data
  > POST /api/roles
  > ![https://i.imgur.com/iycGiIu.png](https://i.imgur.com/iycGiIu.png)

# Admin Panel

- [YashKumarVerma/camicroscope-admin](https://github.com/YashKumarVerma/camicroscope-admin) : to be transferred to **[@caMicroScope](https://github.com/camicroscope)**

  > HomePage
  > ![https://i.imgur.com/xwhca6p.png](https://i.imgur.com/xwhca6p.png)

> Page with all resources
> ![https://i.imgur.com/qDrQ5RZ.png](https://i.imgur.com/qDrQ5RZ.png)

> Interface to edit roles
> ![https://i.imgur.com/gBafs45.png](https://i.imgur.com/gBafs45.png)

> Single button to update the live configurations
> ![https://i.imgur.com/sCzzXNg.png](https://i.imgur.com/sCzzXNg.png)

# Cache System

> A modern NodeJS package to cache any function with just a **single line of code** - @easy-cache

![EasyCache](https://github.com/YashKumarVerma/easy-cache/blob/master/assets/banner.png?raw=true)

[![Ensure Package Builds](https://github.com/YashKumarVerma/easy-cache/actions/workflows/build.yml/badge.svg)](https://github.com/YashKumarVerma/easy-cache/actions/workflows/build.yml)
[![Tests](https://github.com/YashKumarVerma/easy-cache/actions/workflows/test.yml/badge.svg)](https://github.com/YashKumarVerma/easy-cache/actions/workflows/test.yml)

# EasyCache

## ???? What is EasyCache v1.0?

@easy-cache is a NodeJS module available on NPM to cache any function with just a single line of code. It helps you to boost your application in the cleanest way possible. Allows setting global as well as local cache options. Supports TypeScript as well as JavaScript.

Primary features of @easy-cache are:

- Global configuration options to configure entire application at one place.
- Local configuration option to configure any function as required, overriding the global configs.
- Custom TTL (Time to Live) to customize cache invalidation.
- Built in support for Redis as cache server. (_Others coming in v2.0_)
- Support for javaScript and TypeScript in a single package.
- Single line implementation even in JS where decorators do not exist.
- brutally tested and production ready.

# Usage

The package can be directly used in any nodejs (TypeScript / JavaScript) based backend.

**Step 1**: Install the package

```
npm install @easy-cache/core

# or

yarn add @easy-cache/core
```

> Note: In v1.0, the default cache engine i.e. `redis` is used, and therefore the `redis` package is also installed by default. From v2.0 onwards, there would be 0 dependencies in the core package.

**Step 2**: Initialize the cache system

```javascript
/** Initializing the cache system */
import EasyCache from "@easy-cache/core";

const cacheInstance = new EasyCache({
  // TTL Value in seconds
  defaultTTL: 10,

  // redis connection details
  redisHost: "127.0.0.1",
  redisPassword: "",
  redisPort: 6379,

  // whether to show verbose logs
  debug: false,

  // completely disable the cache globally
  disable: false,
});

/** the function that you want to cache **/
function fullName(fistName, lastName) {
  return `${firstName} ${lastName}`;
}

/** To cache the function, just pass it into the provider with configs..
 *
 * Configurations can be passed into the provider to change the behavior.
 * disable: to turn of cache for given funtion
 * expireAfter: to set a custom expire time for given function
 * debug: to show verbose logs for given function
 */
const cachedFullName = cacheInstance.provider({ debug: true }, fullName);
```

### Using with classes

Since v1.0, EasyCache supports classes as well, using a unique method called decorators.
Simply adding `@EasyCache.Cache({configs})` will cache the following function.

**Example**:

```ts
class MyClass {
  @EasyCacheInstance.Cache({ defaultTTL: 30 })
  public static thisMethodWillBeCached() {
    // some code
    return true;
  }
}
```

The above function would cache with a ttl of 30 seconds.

# Tests

Tests are a vital component of any software project. In @easy-cache,

- the unit tests are written in TypeScript using Mocha and Chai.
- the end to end tests are being written in JavaScript using Mocha and Chai.

A detailed analysis about the above is [posted here](https://medium.com/@yk.verma2000/tdd-design-choices-src-or-dist-7261c7c81cb0).

# Roadmap

### Version 1

- @cache decorator to implement caching for class methods.
- provider to cache orphan functions (without decorators).
- Support for javascript and typescript.
- Redis as a default provider for caching.
- Robust test suite.

### Version 2

- Support for plugins to add any generic provider.
- Support for Postgres, Memcached, etc. as providers on rolling basis.
- Plugins
