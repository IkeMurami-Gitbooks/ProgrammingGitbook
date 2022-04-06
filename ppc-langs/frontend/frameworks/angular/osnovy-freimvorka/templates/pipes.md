# Pipes

Это инструменты обработки данных в шаблонах

```markup
<div>
    <p>{{ someObj | json }}</p>
    <p>{{ someDate | date }}</p>
</div>
```

Найти информацию по пайпам, их параметрам можно в документации, в разделе API, отфильтровав по полю Type = Pipe.

## Create

Можно создавать свои пайпы. Например:

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({name: 'myPipe'})
export class MyTestPipe implements PipeTransform {
    transform(value: number, exponent = 1): number {
        return Math.pow(value, exponent);
    }
}
```

```markup
<p>Super power boost: {{2 | myPipe: 10}}</p>
```

## Pure and inpure pipes

Есть у Pipe флаг pure. По умолчанию, все pipes имеют значения pure=true. Это значит, что такой pipe отслеживает изменения только простых типов. Если поставить флаг pure=false, но pipe начнет отслеживать изменения сложных объектов (например, массивов), но в этом случае мы просядем по производительности.

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
    name: 'myPipe',
    pure: false
})
export class MyInpurePipe implements PipeTransform {
    ...
}
```
