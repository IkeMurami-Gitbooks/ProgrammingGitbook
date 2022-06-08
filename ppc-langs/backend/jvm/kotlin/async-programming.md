# Async Programming

## Handlers & Выполнение кода вне главного потока

```kotlin
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        my_button.setOnClickListener {
            val handler = Handler() // этот инструмент нужен для того, чтобы
            // выполнять код из одного треда в главном (напрямую так делать нельзя)
            // например, обновить текстовое поле во вьюхе
            
            // Запускаем еще один поток
            Thread(Runnable {
                val test = 123
                /*
                doSomething()
                */
                
                // изменяем что-то в главном треде
                handler.post(Runnable {
                    outputView.text = "my text and $test"
                })
            }).start()
        }
    }
}
```

## Async/Await

Введение: [https://habr.com/ru/post/314656/](https://habr.com/ru/post/314656/)

Концептуальный пример:

```kotlin
fun fun1() = async<String> {
    val user = await(repo.getUser())
    user.username
}
```

Еще ссылки:\
[https://medium.com/@nhaarman/a-dive-into-async-await-on-android-5a6699029aa3](https://medium.com/@nhaarman/a-dive-into-async-await-on-android-5a6699029aa3)\
[https://medium.com/@jitesh313/network-calls-kotlin-coroutine-retrofit-2-e058ebc56189](https://medium.com/@jitesh313/network-calls-kotlin-coroutine-retrofit-2-e058ebc56189)

## Выполнение периодических событий

TimerTask

## Coroutines

Ссылки:\
[https://android.jlelse.eu/kotlin-coroutines-and-retrofit-e0702d0b8e8f](https://android.jlelse.eu/kotlin-coroutines-and-retrofit-e0702d0b8e8f)

В app/build.gradle:

```
// Kotlin & Coroutines
implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:$coroutines_version"
implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:$coroutines_version"
```

В коде

```kotlin
Thread(Runnable{
    // your code in background thread
}).start()
```
