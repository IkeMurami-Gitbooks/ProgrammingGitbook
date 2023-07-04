# Project Structure

## Пример структуры:

<pre><code><strong>/myproject
</strong>   /dist
      .gitignore
   /src
      /client
         /components
         /constants
         /containers
         /store
         /pages
         /utils
         index.tsx
         Application.tsx
      /server
      /shared
   /tools
      /deployment
         Dockerfile
         /ci
   /scripts
   /assets
   /bundler-config
      %envName%.js
      common.js
   babel.config.js
   tsconfig.js
   package.json
</code></pre>

dist — собранные бандлы\
src/client — код фронта\
src/server — код бэка\
src/shared — общий код\
scripts — скрипты из package.json\
assets — ресурсы (картинки, иконки, шрифты и тп)\
bundler-config — конфиги webpack'а\
babel.config.js — конфиг транспайлера\
tsconfig.json — ts конфиг

## FSD

FSD — один из архитектурных подходов при разработке Frontend приложений [https://feature-sliced.design/ru/docs/get-started/overview](https://feature-sliced.design/ru/docs/get-started/overview)
