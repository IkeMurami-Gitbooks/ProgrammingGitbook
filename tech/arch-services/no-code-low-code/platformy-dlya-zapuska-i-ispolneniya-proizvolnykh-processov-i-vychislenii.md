# Платформы для запуска и исполнения произвольных процессов и вычислений

Все мечтают о платформах, в которых пишешь маленькие воркеры-кирпичики, строишь в виде графа связи для потоков данных и у тебя все работает. И это позволяет переиспользовать чужие воркеры без написания к ним коннекторов. Тем самым экономим время на запуск сервиса.

У крупных компаний подобные решения могут быть или они есть. В паблике — хз как нагуглить.&#x20;

Начну собирать информацию о подобных сервисах (или близких по функциональности):

* Pipedream — код на Python, Go, JS + куча готовых воркеров для разных сервисов (тг, гугл, дискорд, ...) [https://pipedream.com/](https://pipedream.com/)
* Tempolar – пишем воркеры, а они исполняются в облаке: [https://temporal.io/](https://temporal.io/)
* Budibase (open source) — строим внутренние сервисы (как Pipedream): [https://budibase.com](https://budibase.com)
* [https://zapier.com](https://zapier.com)
* [https://ifttt.com/](https://ifttt.com/)
