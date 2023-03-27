# Struts 2

Struts 2 for Beginners: [https://www.digitalocean.com/community/tutorials/struts-tutorial-for-beginners](https://www.digitalocean.com/community/tutorials/struts-tutorial-for-beginners)

Struts 2 — один из старейших фреймворков для построения Java Web Apps. Базируется на OpenSymphony WebWork библиотеке.

Состоит из следующих концепций:

* **Interceptors** — похожи на [Servlet Filters](https://www.digitalocean.com/community/tutorials/java-servlet-filter-example-tutorial), работают поверх всех запросов к веб-приложению
* **ValueStack** — область хранения данных для обработки клиентских запросов. Object-Graph Navigation Language (**OGNL**) — Expression Language для доступа к данным и манипуляции с ними в ValueStack.
* **Action** — обработка клиентских запросов. Struts 2 позволяет реализовать это несколькими способами:
  * Реализуя интерфейс `com.opensymphony.xwork2.Action`
  * Расширяя класс `com.opensymphony.xwork2.ActionSupport`. Обычно это используется для создания пустого Action-класса для перенаправления на другие ресурсы
  * Через аннотации `@Action` или `@Actions`.
  * Класс, соблюдающий общее соглашение об именовании — заканчивается название класса на `Action` и есть метод `execute()`.
* **Result** — обычно это JSP или HTML страницы, которые создают визуальную часть для ответа. Struts 2 добавляет свои теги.
* **Declarative Architecture and Wiring** — Struts 2 дает два способа связать логику между Action и Result:
  * **Struts XML File** — `struts.xml` в `WEB-INF/classes` директории, где мы настраиваем Actions и Results.
  * **Annotation** — можем использовать [Java Annotations](https://www.digitalocean.com/community/tutorials/java-annotations) для настройки метаданных в классах. Мы можем использовать `@Action` и `@Result` аннотации для настройки Action и Result страниц.

