# OpenTelemetry Concepts

Поддерживалось два open source проекта для сбора телеметрии с сервисов — [OpenCencus](https://opencensus.io) (Google Open Source community project) и [OpenTracing](https://opentracing.io/) (CNCF project). Сейчас эти проекты соединили в один — [OpenTelemetry](https://opentelemetry.io) (OTeL) и развивать будут его, другие архивированы.

## OpenTelemetry Concepts

### Dev

Link: [https://opentelemetry.io/docs/getting-started/dev/](https://opentelemetry.io/docs/getting-started/dev/)

С переходом от монолитов к микросервисам количество событий увеличивается и все труднее следить за взаимодействием компонентов. Основная цель — увеличить **Observability** для разработчиков и администраторов.&#x20;

Для этого нам надо собирать:

* Трейсы
* Метрики
* Логи

Все эти данные мы шлем на Observability Backend: от self-hosted решений (например, [Jaeger](https://www.jaegertracing.io/) и [Zipkin](https://zipkin.io/), [Prometheus](https://prometheus.io/)) до коммерческих SaaS предложений.

### Ops

Link: [https://opentelemetry.io/docs/getting-started/ops/](https://opentelemetry.io/docs/getting-started/ops/)
