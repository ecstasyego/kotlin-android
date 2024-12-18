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
df.schema()
df.rowsCount()
df.columnsCount()
df.size()

// df.columns()
val datacolumn0 = df.columns()[0]
val datacolumn1 = df.columns()[1]
val column0_name = df.columns()[0].name
val column1_name = df.columns()[1].name


// df.rows()
for (row in df.rows()) {
    val name = row["Name"] as String
    val age = row["Age"] as Int
    println("Name: $name, Age: $age")
}

df.rows().toList()
df.columns().toList()

// for dataframe
df.map{it["Name"] as String}
df.map{it["Age"] as Int}
df.filter{(it["Age"] as Int) > 29 }
df.filter{(it["Name"] as String)[0] == 'A'}
df.update("Age"){it as Int + 1}
df.update("Name"){"NAME: " + it as String}
df.update("Age"){it as Int + 1}.update("Name"){"NAME: " + it as String}

// for datacolumn
df["Name"].toList()
df["Age"].toList()
df.get("Name").toList()
df.get("Age").toList()
df["Name"].map{it as String}
df["Name"].map{it.toString().toBoolean()}
df["Name"].map{it.toString().toString()}
df["Name"].filter{(it as String)[0] == 'A'}
df["Age"].map{it as Int}
df["Age"].map{it.toString().toInt()}
df["Age"].map{it.toString().toDouble()}
df["Age"].filter{it as Int > 29}
```


```kotlin
import org.jetbrains.kotlinx.dataframe.api.*

val nameColumn = columnOf("Alice", "Bob", "Charlie")
val ageColumn = columnOf(25, 30, 35)
val heightColumn = columnOf(5.6, 5.8, 5.9)
dataFrameOf("Name", "Age", "Height")(nameColumn, ageColumn, heightColumn)
```

```kotlin
import org.jetbrains.kotlinx.dataframe.api.*

val mmap = mutableMapOf<String, Array<Any>>()
mmap["Name"] = arrayOf("Alice", "Bob", "Charlie")
mmap["Age"] = arrayOf(25, 30, 35)
mmap["Height"] = arrayOf(5.6, 5.8, 5.9)

val nameColumn = columnOf(*mmap["Name"]!!)
val ageColumn = columnOf(*mmap["Age"]!!)
val heightColumn = columnOf(*mmap["Height"]!!)

dataFrameOf("Name", "Age", "Height")(nameColumn, ageColumn, heightColumn)
dataFrameOf(*mmap.keys.toTypedArray())(nameColumn, ageColumn, heightColumn)
dataFrameOf("Name", "Age", "Height")(*mmap.values.indices.map{row -> mmap.values.map{it[row]}}.flatMap{it}.toTypedArray())
dataFrameOf(*mmap.keys.toTypedArray())(*mmap.values.indices.map{row -> mmap.values.map{it[row]}}.flatMap{it}.toTypedArray())
```
