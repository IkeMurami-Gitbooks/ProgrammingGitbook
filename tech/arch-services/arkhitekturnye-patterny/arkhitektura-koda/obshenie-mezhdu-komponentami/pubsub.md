# PubSub

**Паттерн PubSub(Publisher-Subscriber)** является вариацией **Observer**. Взаимодействие между компонентами происходит через канал связи **Event channel**. _Publisher_ отправляет события в event channel. _Subscriber_ подписывается на нужное события и ждет его поступления в event channel.

Ключевым различием между классическим Observer и PubSub является [_**слабая связность(loose coupling)**_](https://medium.com/german-gorelkin/low-coupling-high-cohesion-d36369fb1be9). Publisher и Subscriber в PubSub не знают о существование друг друга, в отличии от Observer и Subject.

Паттерн PubSub подходит для асинхронного взаимодействия различных приложений в системе. В качестве _event channal_ используют вариации брокером, шин событий и пр(broker, message broker,event bus, …)

## Python

Пример использования этого паттерна через пакет PyPubSub: [https://github.com/IkeMurami-Examples/pypubsub-example](https://github.com/IkeMurami-Examples/pypubsub-example)
