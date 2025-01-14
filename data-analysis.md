# Data Analysis
## Data
```kotlin
import kotlin.random.Random

// Map<String, List<Any?>> : List<Map<Any?>>
fun rowsIter(dataMMap: MutableMap<String, List<Any?>>): List<Map<String, Any?>>{
    tailrec fun denestingPair( nestedRow: Any, unnestedRow: MutableList<Any?> = mutableListOf<Any?>() ): MutableList<Any?> {
        if (nestedRow is Pair<*, *>) {
            if (nestedRow.first is Pair<*, *>){
                unnestedRow.add(nestedRow.second)
                return denestingPair(nestedRow.first as Pair<*, *>, unnestedRow)
            } else {
                unnestedRow.add(nestedRow)
            }
        } 
        return unnestedRow.reversed().map{it as Pair<String, Any?>}.toMutableList()
    }
    
    lateinit var nameColumn:List<String> 
    lateinit var listPairs:List<Any?>
    for ( (idx, key) in (0 until dataMMap.keys.size).zip(dataMMap.keys) ) {
        nameColumn = List<String>(dataMMap[key]!!.size){ key as String }
        listPairs = if (idx == 0) nameColumn.zip(dataMMap[key]!!) else listPairs.zip(nameColumn.zip(dataMMap[key]!!))
    }
    return listPairs.map{denestingPair(it as Any).map{it as Pair<String, Any>}}.map{it.toMap()}
}

// Data
val dataMMap = mutableMapOf<String, List<Any?>>(
    "C0" to (0 until 100).toList(),
    "C1" to List(100){Random.nextInt(0, 100)},
    "C2" to List(100){Random.nextInt(0, 100)},
) // Map<String, List<Any?>>
val dataIter = rowsIter(dataMMap) // List<Map<String, Any?>>
dataIter[0]["C0"]
```


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
