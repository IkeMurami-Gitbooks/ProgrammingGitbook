# Errors

## Проблема: error TS7016

```
Error: node_modules/webcola/dist/src/d3v4adaptor.d.ts:3:32 - error TS7016: Could not find a declaration file for module 'd3-drag'. '/Users/o.petrakov/JsProjects/asman-ts/node_modules/webcola/node_modules/d3-drag/dist/d3-drag.js' implicitly has an 'any' type.
  Try `npm i --save-dev @types/d3-drag` if it exists or add a new declaration (.d.ts) file containing `declare module 'd3-drag';`

3 import { drag as d3drag } from 'd3-drag';
```

Решение: [https://pjausovec.medium.com/how-to-fix-error-ts7016-could-not-find-a-declaration-file-for-module-xyz-has-an-any-type-ecab588800a8](https://pjausovec.medium.com/how-to-fix-error-ts7016-could-not-find-a-declaration-file-for-module-xyz-has-an-any-type-ecab588800a8)

Или кратко (если проблема в 3-th parts modules):

```javascript
{
    "compilerOptions": {
        // ...
        "noImplicitAny": false
        // ...    
    }
}
```
