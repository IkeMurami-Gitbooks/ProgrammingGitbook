# Files & Databases

## Список

### Работа с базами

db orm (mssql, postgres, sqlite3,..) [https://github.com/jinzhu/gorm](https://github.com/jinzhu/gorm)

MongoDB driver (old fork?): [https://github.com/globalsign/mgo](https://github.com/globalsign/mgo)

MongoDB official driver: `go get go.mongodb.org/mongo-driver`

Tarantool DB driver: [https://github.com/tarantool/go-tarantool](https://github.com/tarantool/go-tarantool)

Можно использовать в связке:

* Построение SQL-запросов с Prepared Statement: [https://github.com/Masterminds/squirrel](https://github.com/Masterminds/squirrel)
* Выполнение SQL-запросов к базам: [https://github.com/jmoiron/sqlx](https://github.com/jmoiron/sqlx)

### Работа с различными файлами

xml parse\&create: [https://github.com/beevik/etree](https://github.com/beevik/etree)

parse dynamic or unknown json structure: [https://github.com/Jeffail/gabs](https://github.com/Jeffail/gabs)

yaml [https://github.com/shopify-graveyard/yaml](https://github.com/shopify-graveyard/yaml)

sourcemap: [https://gopkg.in/sourcemap.v1](https://gopkg.in/sourcemap.v1)

json (есть и "encoding/json", хз в чем разница и какие преимущества, кроме скорости): [https://github.com/json-iterator/go](https://github.com/json-iterator/go)

## Примеры

### MongoDB

```go
import (
	"go.mongodb.org/mongo-driver/mongo"
	"go.mongodb.org/mongo-driver/mongo/options"
)
// initMongo — Initialize MongoDB Client
func initMongo(ctx context.Context) *mongo.Client {
	// MongoDB https://www.digitalocean.com/community/tutorials/how-to-use-go-with-mongodb-using-the-mongodb-go-driver-ru
	mongoClientOptions := options.Client().ApplyURI("mongodb://localhost:27017/")
	client, err := mongo.Connect(ctx, mongoClientOptions)
	if err != nil {
		return nil
	}

	// Check Connect
	err = client.Ping(ctx, nil)
	if err != nil {
		return nil
	}

	return client
}

// Создаем библиотеку storer и в ней коллекцию sources
client := initMongo(context.TODO())
var collection := client.Database("storer").Collection("sources")
```
