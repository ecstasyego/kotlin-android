```kotlin
USE {
    dependencies {
        implementation("org.jetbrains.kotlinx:dataframe:0.8.0")
    }
}
```

```kotlin
import org.jetbrains.kotlinx.dataframe.api.*

val df = dataFrameOf("Name", "Age")(
    "Alice", 25, 
    "Bob", 30, 
    "Charlie", 35
)
df.rows().toList()
df["Name"].toList()
df["Age"].toList()
df.get("Name").toList()
df.get("Age").toList()
df["Name"].map{it.toString().toBoolean()}
df["Name"].map{it.toString().toString()}
df["Age"].map{it.toString().toInt()}
df["Age"].map{it.toString().toDouble()}
```


```kotlin
import org.jetbrains.kotlinx.dataframe.api.*

val nameColumn = columnOf("Alice", "Bob", "Charlie")
val ageColumn = columnOf(25, 30, 35)
val heightColumn = columnOf(5.6, 5.8, 5.9)
val df = dataFrameOf("Name", "Age", "Height")(
    nameColumn, ageColumn, heightColumn
)

```
