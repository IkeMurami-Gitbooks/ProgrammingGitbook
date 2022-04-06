# Подключение шрифтов в проект

На примере Angular и шрифта Roboto, подключим шрифт глобально.&#x20;

Сайт шрифта: [https://fonts.google.com/specimen/Roboto#standard-styles](https://fonts.google.com/specimen/Roboto#standard-styles)

Выбираем на сайте какой-то шрифт (жмем Select this font, и у нас откроется боковое меню с инструкцией как установить шрифт к себе). Выбираем import, копируем то, что в теге style.

Вставляем импорт в styles.scss (файл в angular, отвечающий за импорт и применение глобальных стилей) и дописываем применение этого шрифта ко всему html-документу (эта строчка приведена там же на сайте):

```css
html {
    font-family: 'Roboto', sans-serif;
}
```



