# Redis

Примеры: [https://github.com/IkeMurami/dockerize-that/tree/master/Databases/Redis](https://github.com/IkeMurami/dockerize-that/tree/master/Databases/Redis)

Паттерны над redis: [https://redis.io/docs/latest/develop/clients/patterns/](https://redis.io/docs/latest/develop/clients/patterns/)

Клиенты:

* JS/TS
  * [https://redis.js.org/#node-redis-installation](https://redis.js.org/#node-redis-installation)
  * [https://www.npmjs.com/package/ioredis](https://www.npmjs.com/package/ioredis)

### Базовые команды и типы

В Redis “коллекция” = это просто ключ определенного типа. Они появляются автоматически после первой записи

Коллекции:

* String — коллекция строк или байтов
* List
* Set
* Hash
* Sorted Set

На примере с node-redis

```js
import { createClient } from 'redis';

const client = createClient({
  url: 'redis://localhost:6379'
});

client.on('error', (err) => console.error('Redis error:', err));

async function main() {
  await client.connect();

  // String
  await client.set('session:1', 'active');
  console.log(await client.get('session:1'));

  // List (rPush / lPush, rRange / lRange)
  await client.rPush('tasks', ['task1', 'task2']);
  console.log(await client.lRange('tasks', 0, -1));

  // Set
  await client.sAdd('users', ['alice', 'bob']);
  console.log(await client.sMembers('users'));

  // Hash
  await client.hSet('user:1', {
    name: 'Ivan',
    age: '30'
  });
  console.log(await client.hGetAll('user:1'));

  // Sorted Set
  await client.zAdd('rating', [
    { score: 10, value: 'player1' },
    { score: 20, value: 'player2' }
  ]);
  console.log(await client.zRangeWithScores('rating', 0, -1));

  await client.destroy();
}

main();

```

Проверить тип ключа

```js
const type = await client.type('user:1');
console.log(type); // hash
```

Удалить коллекцию

```js
await client.del('tasks')
```
