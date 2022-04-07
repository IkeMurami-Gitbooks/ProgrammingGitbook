# Material Design Support

Версия для angular: [https://material.angular.io/](https://material.angular.io)



### Install

```
$ ng add @angular/material
```

### Usage

Добавляем в `styles.scss`  (иначе стили не будут применяться)

```css
@import "~@angular/material/prebuilt-themes/indigo-pink.css"; 
```

### Sidenav

Есть два типа элементов, хз в чем отличие

* `mat-drawer-container` —&#x20;
* `mat-sidenav-container` —&#x20;

Есть следующие полезные атрибуты:

* mode
  * push — новая область перекрывает старую, но область на заднем плане видно
  * over — новая область перекрывает старую. Область на заднем плане не видно
  * side — новая область не перекрывает старую, можно работать с двумя областями (копировать, вставлять и тп)
* opened — открыта ли область по умолчанию
* position — start | end — слева или справа новая область
