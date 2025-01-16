
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

```kotlin
interface Listener {
    fun onClick()
    fun onTouch()
}

object Singleton: Listener {
    var count = 0
    fun increment() { count++ }
    fun decrement() { count-- }

    override fun onClick() { println(count) }
    override fun onTouch() { println(count) }
    
}

Singleton.increment()
Singleton.decrement()
Singleton.count
Singleton.onClick()
Singleton.onTouch()
```

```kotlin
open class Listener {
    open fun onClick(){}
    open fun onTouch(){}
}

object Singleton: Listener() {
    var count = 0
    fun increment() { count++ }
    fun decrement() { count-- }

    override fun onClick() { println(count) }
    override fun onTouch() { println(count) }
    
}

Singleton.increment()
Singleton.decrement()
Singleton.count
Singleton.onClick()
Singleton.onTouch()
```
