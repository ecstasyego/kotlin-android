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
