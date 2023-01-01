# Service Worker API

## About

Сервис-воркер это файл JavaScript, который регистрируется на конкретном источнике (веб-сайте или части сайта на конкретном домене) при обращении браузером. После регистрации, он может управлять страницами на этом источнике. Воркер находится между загруженной страницей и сетевым соединением, перехватывая сетевые запросы источника.

Когда worker перехватывает запрос, он может делать многие вещи (смотри [идеи для использования сервис-воркеров](https://developer.mozilla.org/en-US/docs/Web/API/Service\_Worker\_API#other\_use\_case\_ideas)), но классический пример это сохранение сетевых ответов и затем доступ к ним при запросе, вместо запросов по сети. В результате, это позволяет сделать веб-сайт полностью работающим в офлайне.

Service worker фактически выполняет роль прокси-сервера, находящегося между веб-приложением и браузером, а также сетью (если доступна). Он позволяет (кроме прочего) описывать корректное поведение веб-приложения в режиме офлайн, перехватывать запросы сети и принимать соответствующие меры, основываясь на доступности сети, и обновлять данные, находящиеся на сервере при доступе к нему. Также они имеют доступ к push-уведомлениям и API для фоновой синхронизации.

Service worker запускается в контексте worker'ов, поэтому он не имеет доступа к DOM и работает в потоке отдельном от основного потока JavaScript, управляющего вашим приложением, а следовательно — не блокирует его. Он призван быть полностью асинхронным; как следствие, синхронные API, такие как [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) и [localStorage](https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage), в Service Worker'е использовать нельзя.

## Пример сервис воркер

Первое,что мы делаем, это проверка на то, что `serviceWorker` доступен в объекте [`Navigator`](https://developer.mozilla.org/ru/docs/Web/API/Navigator). Если этот так, тогда мы знаем, что как минимум, базовые функции сервис-воркера доступны. Внутри проверки мы используем метод [`ServiceWorkerContainer.register()`](https://developer.mozilla.org/ru/docs/Web/API/ServiceWorkerContainer/register) для регистрации сервис-воркера, находящегося в файле `sw.js` на текущем источнике, таким образом, он может управлять страницами в текущей или внутренних директориях. Когда Promise выполнится, сервис-воркер считается зарегистрированным.

```javascript
  // Регистрация сервис-воркера для обеспечения оффлайн доступности сайта

  if('serviceWorker' in navigator) {
    navigator.serviceWorker
             .register('/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js')
             .then(function() { console.log('Service Worker зарегистрирован'); });
  }
```

Примечание: Путь к файлу `sw.js` указан относительно корня сайта, а не JavaScript файла, содержащего основной код.

### Устанавливаем сервис воркер

В следующий раз, когда страница с сервис-воркером будет запрошена (например когда страница будет перезагружена), сервис-воркер запустится на этой странице и начнет контролировать её. Когда это произойдет, событие `install` будет вызвано в сервис-воркере; вы можете написать код внутри сервис-воркера, который будет вызван в процессе установки.

Давайте взглянем на файл сервис-воркера [sw.js](https://github.com/mdn/learning-area/blob/master/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js). Вы можете видеть, что слушатель события `install` зарегистрирован на `self`. Ключевое слово `self` это способ ссылки на глобальную область видимости сервис-воркера из файла с сервис-воркером.

Внутри обработчика `install` мы используем метод [`ExtendableEvent.waitUntil()`](https://developer.mozilla.org/ru/docs/Web/API/ExtendableEvent/waitUntil), доступном в объекте события, чтобы сигнализировать, что работа продолжается, и браузер не должен завершать установку, пока все задачи внутри блока не будут выполнены.

Здесь мы видим Cache API в действии. Мы используем метод [`CacheStorage.open()`](https://developer.mozilla.org/ru/docs/Web/API/CacheStorage/open) для открытия нового объекта кэша, в котором ответы могут быть сохранены (похоже на объект хранилища IndexedDB). Обещание выполнится с объектом [`Cache`](https://developer.mozilla.org/ru/docs/Web/API/Cache), представляющим собой кэш `video-store` . Затем мы используем метод [`Cache.addAll()`](https://developer.mozilla.org/ru/docs/Web/API/Cache/addAll) для получения ресурсов и добавления ответов в кэш.

```javascript
self.addEventListener('install', function(e) {
 e.waitUntil(
   caches.open('video-store').then(function(cache) {
     return cache.addAll([
       '/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/',
       '/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.html',
       '/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.js',
       '/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/style.css'
     ]);
   })
 );
});
```

### Отвечаем на последующие запросы

Когда сервис-воркер зарегистрирован и установлен на странице HTML и сопутствующие ресурсы добавлены в кэш, все практически готово. Нужно сделать еще одну вещь - написать код для ответа на дальнейшие сетевые запросы.

Мы добавим еще один слушатель к сервис-воркеру в глобальной области видимости, который запускает функцию-обработчик при событии `fetch`. Это происходит всякий раз, когда браузер делает запрос ресурса в директорию, где зарегистрирован сервис-воркер.

Внутри обработчика, мы сначала выводим в консоль URL запрашиваемого ресурса. Затем отдаем особый ответ на запрос, используя метод `FetchEvent.respondWith()`.

Внутри блока мы используем [`CacheStorage.match()`](https://developer.mozilla.org/ru/docs/Web/API/CacheStorage/match) чтобы проверить, можно ли найти соответствующий запрос (т.е. совпадение по URL) в кэше. Обещание возвращает найденный ответ или `undefined`, если ничего не нашлось.

Если совпадение нашлось, то просто возвращаем его как особый ответ. В противном случае, используем [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) для запроса ресурса из сети.

```javascript
self.addEventListener('fetch', function(e) {
  console.log(e.request.url);
  e.respondWith(
    caches.match(e.request).then(function(response) {
      return response || fetch(e.request);
    })
  );
});
```

## Security

### Attack

Загружаем наш html с js с регистрацией сервис воркера (если есть возможность — в корень)

Добавляем eventListener на fetch с отправкой нам уведомления

Обращаемся к html -> видим в консоли, зарегистрирован ли service worker (можно проверить в about:debugging#workers)

Далее, можем перехватывать и менять запросы и ответы к серверу

Поидее, можем таким манипулировать через stored xss (или заманивая пользователей на наш html загруженный)

### Defence

* Content-Disposition: attachment&#x20;
* дополнительная директория : https://example.com/subdir\_y3ugbdcy2yfg8efjdnms/uploaded\_files.html



## Другие ваврианты использования Service Workers

[link](https://developer.mozilla.org/ru/docs/Web/API/Service\_Worker\_API#%D0%B4%D1%80%D1%83%D0%B3%D0%B8%D0%B5\_%D0%B2%D0%B0%D1%80%D0%B8%D0%B0%D0%BD%D1%82%D1%8B\_%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F)
