# Модули

Кидаем в корень проекта.

modules.d.ts — сможем импортировать картинки как объекты:

```typescript
declare module '*.svg';
declare module '*.png';
declare module '*.json';
```

global.d.ts:

```typescript
declare global {
   namespace Test {
        interface Some {
            test?: string;
        }
    }
}
```
