```kotlin
{
    val Singleton = object {
        var count = 0
        fun onClick() { count++; println(count) }
        fun onTouch() { count--; println(count) }
    }

    Singleton.onClick()
    Singleton.onTouch()
}()
```


```kotlin
interface Listener {
    var count:Int
    fun onClick()
    fun onTouch()
}

val Singleton = object : Listener {
    override var count = 0
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}

Singleton.onClick()
Singleton.onTouch()
```


```kotlin
open class Listener {
    var count = 0
    open fun onClick(){}
    open fun onTouch(){}
}

val Singleton = object : Listener() {
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}

Singleton.onClick()
Singleton.onTouch()
```
