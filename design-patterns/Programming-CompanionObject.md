```kotlin
class ApplicationComponent {
    companion object {
        const val CONSTANT = 9999

        fun newInstance():ApplicationComponent {
            val applicationComponent = ApplicationComponent()
            return applicationComponent
        }
    }
}

ApplicationComponent.CONSTANT
ApplicationComponent.newInstance()
```
