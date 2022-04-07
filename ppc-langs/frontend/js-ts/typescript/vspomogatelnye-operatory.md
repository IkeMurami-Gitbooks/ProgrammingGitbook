# Вспомогательные операторы

```typescript
// keyof — получение имен полей 

interface Person {
    name: string
    age: number
}

type PersonKeys = keyof Person  // 'name' | 'age'

let key: PersonKeys = 'name'



// Создание типов из уже имеющихся — операторы Exclude и Pick
type User = {
    _id: number
    name: string
    email: string
    createdAt: Date
}

// Создаем новый тип, исключай поля _id и createdAt
type UserKeysNoMeta1 = Exclude<keyof User, '_id' | 'createdAt'>   // 'name' | 'email'
type UserKeysNoMeta2 = Pick<User, 'name' | 'email'>   // 'name' | 'email'


```
