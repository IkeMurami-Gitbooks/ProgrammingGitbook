# DOM API & элементы веб-браузера

## Tools

Просмотреть DOM документа: [https://software.hixie.ch/utilities/js/live-dom-viewer/](https://software.hixie.ch/utilities/js/live-dom-viewer/)

## Элементы веб-браузера

![](<../../../.gitbook/assets/изображение (4).png>)

* Окно - это вкладка браузера, в которую загружается веб-страница; это представлено в JavaScript объектом [`Window`](https://developer.mozilla.org/ru/docs/Web/API/Window). Используя методы, доступные для этого объекта, вы можете делать такие вещи, как возврат размера окна (см. [`Window.innerWidth`](https://developer.mozilla.org/ru/docs/Web/API/Window/innerWidth) и [`Window.innerHeight`](https://developer.mozilla.org/ru/docs/Web/API/Window/innerHeight)), манипулировать документом, загруженным в этот window, хранить данные, специфичные для этого документа на стороне клиента (например, используя локальную базу данных или другой механизм хранения), присоединить обработчик событий ([event handler](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building\_blocks/Events#A\_series\_of\_fortunate\_events)) к текущему окну и многое другое.
* Навигатор представляет состояние и идентификатор браузера (т. е. пользовательский агент), как он существует в Интернете. В JavaScript это представлено объектом [`Navigator`](https://developer.mozilla.org/ru/docs/Web/API/Navigator). Вы можете использовать этот объект для извлечения таких вещей, как геолокационная информация, предпочтительный язык пользователя, медиапоток с веб-камеры пользователя и т. д.
* Документ (представленный DOM в браузерах) представляет собой фактическую страницу, загруженную в окно, и представлен в JavaScript объектом [`Document`](https://developer.mozilla.org/ru/docs/Web/API/Document). Вы можете использовать этот объект для возврата и обработки информации о HTML и CSS, содержащей документ, например, получить ссылку на элемент в DOM, изменить его текстовый контент, применить к нему новые стили, создать новые элементы и добавить их в текущий элемент как дочерний элемент, или даже вообще удалить его.

## DOM API

### Термины

* **Element node**: элемент, как он существует в DOM.
* **Root node**: верхний узел в дереве, который в случае `HTML` всегда является узлом HTML (другие словари разметки, такие как SVG и пользовательский XML, будут иметь разные корневые элементы)..
* **Child node**: узел _непосредственно_ внутри другого узла. Например, `IMG` является дочерним элементом `SECTION` в приведенном выше примере.
* **Descendant node** (узел потомок): узел внутри дочернего элемента. Например, `IMG` является дочерним элементом `SECTION` в приведенном выше примере, и он также является потомком для родителя `SECTION`. `IMG` не является ребенком `BODY`, так как он находится на двух уровнях ниже дерева в дереве, но он является потомком `BODY`.
* **Parent node**: узел, в котором текущий узел. Например, `BODY` является родительским узлом `SECTION` в приведенном выше примере.
* **Sibling nodes** (одноуровневый узел): узлы, которые расположены на одном уровне в дереве DOM. Например, `IMG` и `P` являются братьями и сестрами в приведенном выше примере.
* **Text node**: узел, содержащий текстовую строку.

### Примеры взаимодействия с DOM

```javascript
var link = document.querySelector('a');  // получить тег <a>
link.textContent = 'Mozilla Developer Network'; // можем менять атрибуты

var links = document.querySelectorAll('a'); // возвращается NodeList

/*
Старые интерфейсы (они тоже работают, но те, что выше, более соременные):
Document.getElementById()
Document.getElementByTagName()
*/

// Находим элемент
var sect = document.querySelector('section');
// Создаем элемент <p>
var para = document.createElement('p');
para.textContent = 'We hope you enjoyed the ride.';
// Добавляем в <section>
sect.appendChild(para);
```
