---
description: Как писать хороший код
---

# Guidelines

## Организация проекта

* Использовать Feature-Sliced Design подход
* Следовать SOLID
  * When using third party packages except Next.js App Router, UI and utilities like `lodash-es` always create wrappers for them with types used in application to make it easier to replace them with other packages later

## Javascript

* Use constructor dependency injection
* Prefer early returns to `if`/`else` statements
* When defining a function always make it arrow function
* Use `AbortController` for event listeners and actions that could be aborted
* Use optional chaining when possible instead of `if`/`else` statements

## Typescript

* Don't use runtime constructs that aren't part of ECMAScript like namespaces and enums
* When importing types use `import type` and inline type imports
* Use types instead of interfaces to avoid declaration merging
* Always include file extension `.js` for relative imports
* Instead of using `private` modifier use ECMAScript native private properties
* Don't use `public` and `protected` modifiers

## React

* Don't import React as a namespace `import * as React from 'react'` and don't use default export `import React from 'react'`
* When registering multiple event listeners inside `useEffect` use `AbortController` for cleanup
* Avoid prop drilling
* For computed grids and lists use `@tanstack/react-virtual` for virtualization
* Always import `clsx` for conditional class names as `import { clsx } from 'clsx'`

## State managers

* Use MobX-State-Tree for state management
