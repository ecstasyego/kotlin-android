
## groupby
```kotlin
val table = mutableListOf<Map<String, Any>>()
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "B", "price" to 435))
table.add(mapOf("name" to "B", "price" to 435))
table.add(mapOf("name" to "C", "price" to -1))

val latestRow = table.groupBy{it["name"]}.map{it.value.last()}
val exceptForLatestRow = table.groupBy{it["name"]}.map { (_, group) -> group.dropLast(1) }.filter{it.size != 0}
exceptForLatestRow
```


## sortedby
```kotlin
val table = mutableListOf<Map<String, Any>>()
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "A", "price" to 10321))
table.add(mapOf("name" to "B", "price" to 435))
table.add(mapOf("name" to "B", "price" to 435))
table.add(mapOf("name" to "C", "price" to -1))

table.sortedBy{it["price"] as Int}
```
