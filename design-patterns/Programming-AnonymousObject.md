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
