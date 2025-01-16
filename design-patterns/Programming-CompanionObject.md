```kotlin
class ApplicationComponent {
    companion object {
        const val CONSTANT = 9999

        fun newInstance():ApplicationComponent {
            val component = ApplicationComponent()
            return component
        }
    }
}

ApplicationComponent.CONSTANT
ApplicationComponent.newInstance()
```
