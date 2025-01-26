## Examples
### Usage: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.widget.HorizontalScrollView
import android.widget.LinearLayout
import android.widget.ScrollView
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val mainLayout = LinearLayout(this)
        val horizontalScrollView = HorizontalScrollView(this)
        val scrollView = ScrollView(this)
        val tableLayout = TableLayout(this).apply{
            layoutParams = TableLayout.LayoutParams(
                TableLayout.LayoutParams.MATCH_PARENT,
                TableLayout.LayoutParams.WRAP_CONTENT
            )
            setPadding(16, 16, 16, 16)
        }

        scrollView.addView(tableLayout)
        horizontalScrollView.addView(scrollView)
        mainLayout.addView(horizontalScrollView)
        setContentView(mainLayout)


        // Database
        val dbFile = applicationContext.getDatabasePath("historyDB")
        if (dbFile.exists()) {dbFile.delete()}
        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        // Data
        val upperCases: List<String> = listOf("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")
        val lowerCases: List<String> = upperCases.map{ it.lowercase() }
        var data = rowsIter(mutableMapOf<String, List<Any?>>(
            "C0" to (0 until 100).toList(),
            "C1" to List(100){ upperCases[Random.nextInt(0, 26)]},
            "C2" to List(100){ lowerCases[Random.nextInt(0, 26)]},
        ))

        // UI Update on background
        Thread(Runnable {
            // [DATA] DAO DELETE
            db.historyDao().delete()
            data.forEach{datum ->
                // [DATA] DAO INSERT
                db.historyDao().insert(
                    History(datum["C0"] as Int, datum["C1"] as String, datum["C2"] as String)
                )
            }

            // UI
            val rows = mutableListOf<TableRow>().apply{
                add(
                    TableRow(this@MainActivity).apply{
                        addView(TextView(this@MainActivity).apply { text = "INDEX" })
                        addView(TextView(this@MainActivity).apply { text = "UID" })
                        addView(TextView(this@MainActivity).apply { text = "EXPRESSION" })
                        addView(TextView(this@MainActivity).apply { text = "RESULT" })
                    }
                ) // columns
            }

            // [DATA] DAO GET
            val daolist = db.historyDao().get().reversed()
            for ( (idx, dao) in (0 until daolist.size).zip(daolist)){
                rows.add(
                    TableRow(this).apply{
                        addView(TextView(this@MainActivity).apply { text = idx.toString() }) // INDEX
                        addView(TextView(this@MainActivity).apply { text = dao.uid.toString() } ) // data
                        addView(TextView(this@MainActivity).apply { text = dao.expression.toString() } ) // data
                        addView(TextView(this@MainActivity).apply { text = dao.result.toString() } ) // data
                    }
                )
            }

            // UI ATTACH
            runOnUiThread {
                rows.forEach { tableLayout.addView(it) }
            }
        }).start()
    }

}


@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    fun delete()

    @Query("SELECT * FROM history")
    fun get(): List<History>

    @Insert
    fun insert(history: History)
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)

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
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
plugins {
    id("kotlin-kapt") // for Room annotation processing
}

android {
    kapt {
        correctErrorTypes = true
    }
}

dependencies {
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br>

### Usage: *.xml(findViewById)
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/table_layout.xml
│   │       ├── res/layout/row_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.LayoutInflater
import android.widget.LinearLayout
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.widget.NestedScrollView
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val nestedScrollView = LayoutInflater.from(this).inflate(R.layout.table_layout, mainLayout, false) as NestedScrollView
        val tableLayout = nestedScrollView.getChildAt(0) as TableLayout

        // Database
        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        // Data
        val upperCases: List<String> = listOf("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")
        val lowerCases: List<String> = upperCases.map{ it.lowercase() }
        var data = rowsIter(mutableMapOf<String, List<Any?>>(
            "C0" to (0 until 100).toList(),
            "C1" to List(100){ upperCases[Random.nextInt(0, 26)]},
            "C2" to List(100){ lowerCases[Random.nextInt(0, 26)]},
        ))

        Thread(Runnable {
            // [DATA] DAO DELETE
            db.historyDao().delete()
            data.forEach{datum ->
                // [DATA] DAO INSERT
                db.historyDao().insert(
                    History(datum["C0"] as Int, datum["C1"] as String, datum["C2"] as String)
                )
            }

            // UI
            val rows = mutableListOf<TableRow>().apply {
                val tableColumnRow = LayoutInflater.from(this@MainActivity).inflate(R.layout.row_layout, null, false) as TableRow
                tableColumnRow.apply {
                    tableColumnRow.findViewById<TextView>(R.id.textView00).text = "UID"
                    tableColumnRow.findViewById<TextView>(R.id.textView01).text = "EXPRESSION"
                    tableColumnRow.findViewById<TextView>(R.id.textView02).text = "RESULT"
                }
                add(tableColumnRow)
            }

            // [DATA] DAO GET
            val daoList = db.historyDao().get().reversed()
            daoList.forEach {
                val tableRow = LayoutInflater.from(this).inflate(R.layout.row_layout, null, false) as TableRow
                tableRow.findViewById<TextView>(R.id.textView00).text = it.uid.toString()
                tableRow.findViewById<TextView>(R.id.textView01).text = it.expression
                tableRow.findViewById<TextView>(R.id.textView02).text = it.result
                rows.add(tableRow)
            }

            // [DATA] DAO GET
            runOnUiThread {
                rows.forEach { tableLayout.addView(it) }
                mainLayout.addView(nestedScrollView)
            }

        }).start()

    }
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    fun delete()

    @Query("SELECT * FROM history")
    fun get(): List<History>

    @Insert
    fun insert(history: History)
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)

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
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</LinearLayout>
```

`table_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.core.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TableLayout
        android:id="@+id/tableLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:stretchColumns="1"
        android:orientation="vertical">
    </TableLayout>
</androidx.core.widget.NestedScrollView>
```

`row_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<TableRow xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textView00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/textView01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/textView02"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
</TableRow>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
plugins {
    id("kotlin-kapt") // for Room annotation processing
}

android {
    kapt {
        correctErrorTypes = true
    }
}

dependencies {
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```



