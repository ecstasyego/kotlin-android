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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import android.view.Gravity
import android.view.View
import android.widget.HorizontalScrollView
import android.widget.LinearLayout
import android.widget.ScrollView
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat
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
    lateinit var mainLayout: QueryView
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainLayout = QueryView(this)
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

        // UI Update on background
        Thread(Runnable {
            // [DATA] DAO DELETE
            db.historyDao().delete()

            // [DATA] DAO INSERTALL
            val data = dataLoader()
            db.historyDao().insertAll(
                data.map{ row -> History(row["C0"] as Int, row["C1"] as String, row["C2"] as String) }.toList()
            )

            // [DATA] DAO INSERT
            data.forEach{row ->
                db.historyDao().insert(
                    History(null, row["C1"] as String, row["C2"] as String)
                )
            }

            // UI
            val rows = mutableListOf<TableRow>().apply{
                add(
                    TableRow(this@MainActivity).apply{
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "INDEX" })
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "UID" })
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "EXPRESSION" })
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "RESULT" })
                    }
                ) // columns
            }

            // [DATA] DAO GET
            val daolist = db.historyDao().get().reversed()
            for ( (idx, dao) in (0 until daolist.size).zip(daolist)){
                rows.add(
                    TableRow(this).apply{
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = idx.toString() }) // INDEX
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.uid.toString() } ) // data
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.expression.toString() } ) // data
                        addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.result.toString() } ) // data
                    }
                )
            }

            // UI ATTACH
            runOnUiThread {
                rows.forEach { mainLayout.tableLayout.addView(it) }
            }
        }).start()
    }

    private fun dataLoader(): List<Map<String, Any?>> {
        val upperCases: List<String> = listOf("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")
        val lowerCases: List<String> = upperCases.map{ it.lowercase() }
        return rowsIter(mutableMapOf<String, List<Any?>>(
            "C0" to (0 until 100).toList(),
            "C1" to List(100){ upperCases[Random.nextInt(0, 26)]},
            "C2" to List(100){ lowerCases[Random.nextInt(0, 26)]},
        ))
    }

}

class QueryView @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0, defStyleRes: Int = 0) : LinearLayout(context, attrs, defStyleAttr, defStyleRes) {
    val tableLayout = TableLayout(context)
    val horizontalScrollView = HorizontalScrollView(context)
    val scrollView = ScrollView(context)

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        orientation = VERTICAL

        horizontalScrollView.apply{ id = View.generateViewId() }
        scrollView.apply{ id = View.generateViewId() }
        tableLayout.apply{
            id = View.generateViewId()
            layoutParams = TableLayout.LayoutParams(TableLayout.LayoutParams.MATCH_PARENT, TableLayout.LayoutParams.WRAP_CONTENT)
            setPadding(16, 16, 16, 16)
        }

        scrollView.addView(tableLayout)
        horizontalScrollView.addView(scrollView)
        addView(horizontalScrollView)
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

    @Insert
    fun insertAll(histories: List<History>)
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
