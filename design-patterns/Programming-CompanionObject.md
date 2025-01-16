```kotlin
class ApplicationComponent {
    companion object {
        const val CONSTANT = 9999

        fun newInstance():Any {
            return "Hello, World!"
        }
    }
}

ApplicationComponent.CONSTANT
ApplicationComponent.newInstance()
```
