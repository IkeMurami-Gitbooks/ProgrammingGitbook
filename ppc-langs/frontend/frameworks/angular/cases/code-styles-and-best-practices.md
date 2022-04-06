# Code Styles And Best Practices

## 1 Про архитектуру

### Спросил в чате Angular Ru:

"Добрый день, подскажите пример «правильной» (той, которую придерживаются в маленьких и средних размеров проектов) структуры проекта angular (может сможете поделиться style guide, принятой в вашей команде разработки). Я придерживался следующей:"

```
src/
    app/
        core/
            services…
        module1/
            component1/
            component2/
        module2/
        …
    main.ts
angular.json
```

но столкнулся с тем, что мне понадобились какие-то части одинаковые в нескольких компонентах и просто стал путаться уже что где (в компонентах отрисовывал рабочие области по тематике)

Недавно наткнулся на следующую структуру:

```
src/
    app/
        core/
            components/
            configs/
            interceptors/
            logger/
            services/
            core.module.ts
        modules/
            globalmodule1/
            globalmodule2/
        share/
            components/
            helpers/
            services/
            …
            share.module.ts
        app.module.ts
        …
    main.ts
angular.json
```

т.е они создали отдельные модули для кора и того, что используется в других компонентах и модулях, а остальное засунули в modules.

Выглядит вроде логично, но может это тоже не спасает еще от каких проблем. В общем, хочется какую рецензию от проразработчиков)

### Ответ

* Посмотрите еще Nx, возможно вам понравится как у них сделано
* Вроде норм, у нас похоже [http://github.com/evoytenkoapps/angular-best-practices](http://github.com/evoytenkoapps/angular-best-practices)
* Гляньте проект jHipster ([https://github.com/jhipster/jhipster-sample-app](https://github.com/jhipster/jhipster-sample-app))

## 2 Разница между Core и Shared

Core — модули, сервисы и компоненты отсюда импортируются только один раз

Shared — могут импортироваться несколько раз в разных модулях, сервисах и компонентах

Если есть элемент, который вы хотите использовать в нескольких приложениях, не добавляйте его в Shared — подумайте о создании Angular Library.
