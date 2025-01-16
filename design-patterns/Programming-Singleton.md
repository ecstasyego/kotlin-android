
```kotlin
object Singleton {
    var count = 0
    fun increment() { count++ }
    fun decrement() { count-- }
}

Singleton.increment()
Singleton.decrement()
Singleton.count
```
