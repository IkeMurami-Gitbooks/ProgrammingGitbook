---
description: Используется для декомпозиции типов
---

# Namespaces

Создаем namespace (файл `test.ts`)

```typescript
namespace TestNamespace {
    export type FormState = 'active' | 'inactive'  // По умолчанию все поля и объекты неэкспортируемые (ну будут видны в других объявлениях этого же namespace)
}
```

Подключаем  namespace  в другой скрипт

```typescript
/// <reference path="test.ts" />

namespace TestNamespace {
    const testVal: FormState = 'active'
}

// Обращаемся в конкретный Namespace
console.log(TestNamespace.testVal)
```
