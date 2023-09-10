# Ext JS / Sencha

Это коммерческий фреймворк для разработки веб приложений.

Site: [https://www.sencha.com/products/extjs/thankyou/?mac](https://www.sencha.com/products/extjs/thankyou/?mac)

Попробовать Trial версию:

```
$ npm install -g @sencha/ext-gen
$ ext-gen app -a
$ cd ./my-app
$ npm start
```

Возможная структура сборки веб приложения:

```
/xxx
    /.sencha
        /app
            build-impl.xml
            find-cmd-impl.xml
            init-impl.xml
            watch-impl.xml
            js-impl.xml
            sass-impl.xml
            resources-impl.xml
            slice-impl.xml
            refresh-impl.xml
            page-impl.xml
            resolve-impl.xml
            packager-impl.xml
            production.properties
            build.properties
            testing.properties
            defaults.properties
            ext.properties
            native.properties
            package.properties
            codegen.json
            plugin.xml
            sencha.cfg
        /workspace
            sencha.cfg
            plugin.xml

    /app
    /build
    /docs
    /ext
    /img
    /resources
    /packages/default/sass/config.rb
    .keep
    app.js
    app.json
    build.xml
    index.html
    home.jsp
    index.jsp
    bootstrap.js
    bootstrap.json
```
