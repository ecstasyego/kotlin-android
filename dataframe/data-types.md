
## List<Map<String, Any>>
```kotlin
val mlist = mutableListOf<Map<String, Any>>()
mlist.add(mapOf("C0" to 1, "C1" to 0, "C2" to 3))
mlist.add(mapOf("C0" to 2, "C1" to 3, "C2" to 4))
mlist.add(mapOf("C0" to 0, "C1" to 0, "C2" to 0))

mlist[0]["C0"]; mlist[0]["C1"]; mlist[0]["C2"]
mlist[1]["C0"]; mlist[1]["C1"]; mlist[1]["C2"]
mlist[2]["C0"]; mlist[2]["C1"]; mlist[2]["C2"]

for (row in mlist){
    println(row.values.joinToString())
    println(row["C0"])
    println(row["C1"])
    println(row["C2"])
}
```

## Map<String, List<Any>>
```kotlin
import kotlin.random.Random

val mmap = mutableMapOf<String, List<Any>>()
mmap["C0"] = Array(5) { Random.nextInt(0, 10) }.toList()
mmap["C1"] = Array(5) { Random.nextDouble(-3.14, 3.14) }.toList()
mmap["C2"] = Array(5) { Random.nextBoolean() }.toList()
mmap["C3"] = Array(5) { Random.nextDouble(-3.14, 3.14) }.toList()
for (col in mmap.keys){
    println(mmap[col]!!.joinToString())
}
```


## Map<String, List<Any>> : List<Map<String, Any>>
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

rowsIter(dataMMap)[0]["C0"]
```
