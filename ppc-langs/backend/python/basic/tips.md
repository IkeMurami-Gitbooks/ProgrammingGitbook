# Tips

* То, что не должно меняться, лучше оборачивать в `tuple`
* `print(blabla, file=file, flush=True)` flush - не будет буферизовать данные: без этого принт ждёт добива блока до 4КБ
* Про наследование, mro и super - [https://habr.com/ru/post/62203](https://habr.com/ru/post/62203)
