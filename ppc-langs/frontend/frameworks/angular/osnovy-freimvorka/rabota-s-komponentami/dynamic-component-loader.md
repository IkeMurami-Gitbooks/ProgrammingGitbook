# Dynamic components

Компоненты не всегда обязаны быть определены заранее. Их можно создавать на лету.

## Создаем Anchor directive

```typescript
// src/app/ad.directive.ts
import { Directive, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[adHost]',
})
export class AdDirective {
  constructor(public viewContainerRef: ViewContainerRef) { }
}
```

Инжектим `ViewContainerRef` для доступа к отображению. `@Directive` декоратор резерввирует имя селектора.

## Loading components

```typescript
// src/app/ad-banner.component.ts
...
    template: `
    <ng-template adHost></ng-template>
    `
...

```

Элемент \<ng-template> отлично подходит для динамических компонентов, тк не отображает другие доп данные

...



Короч надо на конкретном примере разбирать в документации [https://angular.io/guide/dynamic-component-loader](https://angular.io/guide/dynamic-component-loader)
