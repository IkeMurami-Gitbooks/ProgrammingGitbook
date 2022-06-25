# React JS

## Создать проект

```
$ npx create-react-app test-project-name
...
Success! Created test-app at /Users/user/JsProjects/test-app
Inside that directory, you can run several commands:

    yarn start
      Starts the development server.
  
    yarn build
      Bundles the app into static files for production.
  
    yarn test
      Starts the test runner.
  
    yarn eject
      Removes this tool and copies build dependencies, configuration files
      and scripts into the app directory. If you do this, you can’t go back!
  
We suggest that you begin by typing:

  cd test-project-name
  yarn start
  
$ cd test-project-name
$ yarn start (or npm start)
```

## Структура проекта

```
/node_modules — зависимости
/public/index.html — главный html документ (по сути ввсе приложение отрисовывается в компонент с id=root)
package.json — описание зависимостей
index.js — по сути главный исполняемый файл, который отрисовывает компонент App и кладет в root
App.js — внутри видим html-подобный код — это JSX.

Так же в index.js видим в узле scripts команды объяленные: запуск приложения (можно и через yarn) 
npm start
```

Еще про структуру проекта и директории:

* /src/components — описывает все UI компоненты
* /src/css — стили
* /src/img — изображения
* /src/test — тесты
* /src/public — главный html документ и скрипт
* /config/ — конфиги
* /src/actions ....

## Разработка

У React компонентный подход: то есть создаем отдельные компоненты, а потом их комбинируем

## Интересные пакеты

React Flow — построение графов — [https://reactflow.dev/](https://reactflow.dev/)

## Security

### ReactJS Specified XSS

В ReactJS встроена защита, от разного рода XSS инъекций (auto escaping):

```jsx
import './App.css';

function App() {
  let XSS_PAYLOAD = "<img src=x onerror='alert(123)'/>";

  return (
    <div className="App">
      <header className="App-header">
        <div>STRING {XSS_PAYLOAD} ESCAPED</div>
      </header>
    </div>
  );
}

export default App;
```

И выглядит это так:

<img src="../../../../.gitbook/assets/image (1).png" alt="" data-size="original">

Однако, для разработчиков оставили способ отрисовать html+js в динамике, если это действительно необходимо:

```javascript
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

PS:  Ну и всякие documentWrite остаются, конечно:

```javascript
const contentWindow = iframe.contentWindow || iframe.contentDocument.document || iframe.contentDocument

contentWindow.document.open()
contentWindow.document.write(htmlBody)
contentWindow.document.close()
```

### Props Injections

В React есть возможность передать в компонент массив с переопределенными атрибутами. Если атакующий может контролировать этот массив, то он сможет определить ключ атрибута `dangerouslySetInnerHTML` и добавить туда свою нагрузку:

```jsx
import './App.css';

function App() {
  const STRONG_PAYLOAD = "<strong>TEST</strong>";
  const XSS_PAYLOAD = "<img src=x onerror='alert(123)'/>";

  const PROPS_INJ_PAYLOAD = {"dangerouslySetInnerHTML": {"__html": STRONG_PAYLOAD}};
  
  return (
    <div className="App">
      <header className="App-header">
        <div {...PROPS_INJ_PAYLOAD} />
      </header>
    </div>
  );
}

export default App;
```

Один только нюанс: это сработает только в том случае, если в `PROPS_INJ_PAYLOAD` не содержится других ключей, иначе React/Webpack выведут ошибку:&#x20;

![](<../../../../.gitbook/assets/image (1) (1).png>)

### Injectable Attributes

```jsx
import './App.css';

function App() {
    const ATTR_PAYLOAD = "javascript:alert(1)";
    
    return (
        <div className="App">
          <header className="App-header">
              <a href={ATTR_PAYLOAD}>Link</a>
          </header>
        </div>
    );
}

export default App;
```

### CSS in JS

У React есть возможность определять стили для компонентов прямо в JS коде, используя JS синтаксис:

```jsx
```

Короч есть какая-та возможность делать css injection. Но я не разобрался. Надо лезть в документацию и смотреть разницу между class и className (один из них безопасный, другой — нет) и как делать Attribute Injection в эти поля.
