## Examples

### Example01: DAO Basic
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
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
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        Thread(Runnable {
            db.historyDao().insert(History(null, "Hello", "World!"))
        }).start()

        Thread(Runnable {
            val historyList:List<History> = db.historyDao().get() 
        }).start()

        Thread(Runnable {
            db.historyDao().delete()
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


### Example02: Cursor
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

import android.annotation.SuppressLint
import android.database.Cursor
import android.os.Bundle
import android.util.Log
import android.view.LayoutInflater
import android.widget.LinearLayout
import android.widget.TableLayout
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.PrimaryKey
import androidx.room.RawQuery
import androidx.room.Room
import androidx.room.RoomDatabase
import androidx.sqlite.db.SimpleSQLiteQuery
import androidx.sqlite.db.SupportSQLiteQuery

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    @SuppressLint("Range")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val tableLayout = LayoutInflater.from(this).inflate(R.layout.table_layout, null, false) as TableLayout

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        Thread( Runnable {
        val query:String = "select * from history"
            val supportSQLiteQuery: SupportSQLiteQuery = SimpleSQLiteQuery(query)
            val cursor: Cursor = db.historyDao().getTableByQuery(supportSQLiteQuery)
                if (cursor.moveToFirst()) {
                    do {
                        val uid = cursor.getInt(cursor.getColumnIndex("uid"))
                        val expression = cursor.getString(cursor.getColumnIndex("expression"))
                        val result = cursor.getInt(cursor.getColumnIndex("result"))

                        Log.i("MainActivity", "history: $uid, $expression, $result")
                    } while (cursor.moveToNext())
                }
            cursor.close()
        }).start()
    }
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao // DAO: Data Access Object
interface HistoryDao {
    @RawQuery
    fun getTableByQuery(query: SupportSQLiteQuery): Cursor
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
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
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/tableLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:stretchColumns="1"
    android:orientation="vertical">
</TableLayout>
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


<br>



### Example03: UI
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
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val tableLayout = LayoutInflater.from(this).inflate(R.layout.table_layout, null, false) as TableLayout

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        Thread(Runnable {
            db.historyDao().insert(History(null, "Hello", "World!"))
            db.historyDao().insert(History(null, "abcde", "ABCDE"))
            db.historyDao().insert(History(null, "GOOD", "BAD"))
        }).start()

        Thread(Runnable {
            val historyList = db.historyDao().get().reversed() // latest
            runOnUiThread {
                historyList.forEach {
                    val tableRow = LayoutInflater.from(this).inflate(R.layout.row_layout, null, false) as TableRow
                    tableRow.findViewById<TextView>(R.id.textView00).text = it.uid.toString()
                    tableRow.findViewById<TextView>(R.id.textView01).text = it.expression
                    tableRow.findViewById<TextView>(R.id.textView02).text = it.result
                    tableLayout.addView(tableRow)
                }
            }
            mainLayout.addView(tableLayout)
        }).start()

        Thread(Runnable {
            db.historyDao().delete()
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
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/tableLayout"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:stretchColumns="1"
    android:orientation="vertical">
</TableLayout>
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


