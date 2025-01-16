```kotlin
object {
    var count = 0
    fun onClick() { count++; println(count) }
    fun onTouch() { count--; println(count) }
}.onClick()

object {
    var count = 0
    fun onClick() { count++; println(count) }
    fun onTouch() { count--; println(count) }
}.onTouch()
```


```kotlin
interface Listener {
    var count:Int
    fun onClick()
    fun onTouch()
}

object : Listener {
    override var count = 0
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}.onClick()

object : Listener {
    override var count = 0
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}.onTouch()
```


```kotlin
open class Listener {
    var count = 0
    open fun onClick(){}
    open fun onTouch(){}
}

object : Listener() {
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}.onClick()

object : Listener() {
    override fun onClick() { count++; println(count) }
    override fun onTouch() { count--; println(count) }
}.onTouch()
```
