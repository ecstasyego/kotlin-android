## Coroutine
### Activity
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
import android.widget.LinearLayout
import androidx.activity.ComponentActivity
import androidx.lifecycle.lifecycleScope
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase
import androidx.room.withTransaction
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        lifecycleScope.launch(Dispatchers.IO) {
            db = Room.databaseBuilder(
                applicationContext,
                AppDatabase::class.java,
                "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
            ).build()

            db.withTransaction{
                db.historyDao().spndInsert(History(null, "Hello", "World!")) // Insert data into the database using coroutines
                db.historyDao().spndGet() // Query the database
                db.historyDao().spndDelete() // Delete all records from the database
            }

            db.runInTransaction{
                db.historyDao().insert(History(null, "Hello", "World!")) // Insert data into the database using coroutines
                db.historyDao().get() // Query the database
                db.historyDao().delete() // Delete all records from the database
            }

            val historyList: List<History> = db.historyDao().get() // Query the database
            withContext(Dispatchers.Main) {
                // Process the list, update UI, etc.
            }

        }
    }
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    suspend fun spndDelete()

    @Query("SELECT * FROM history")
    suspend fun spndGet(): List<History>

    @Insert
    suspend fun spndInsert(history: History)

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


### with Fragment
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
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.lifecycle.lifecycleScope
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext

class MainActivity : AppCompatActivity() {
    lateinit var db: AppDatabase
    lateinit var repository: Repository

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply {
            addView( FrameLayout(this@MainActivity).apply {id = View.generateViewId()} )
        }
        setContentView(main_layout)

        lifecycleScope.launch(Dispatchers.IO) {
            db = Room.databaseBuilder(
                applicationContext,
                AppDatabase::class.java,
                "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
            ).build()
            repository = Repository(db)

            repository.insertHistory(History(null, "Hello", "World!"))
            repository.getHistoryList() // Query the database
            repository.deleteAllHistory() // Delete all records from the database

            val historyList: List<History> = repository.getHistoryList() // Query the database
            withContext(Dispatchers.Main) {
                // Process the list, update UI, etc.
            }
        }

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()

    }
}

class MainFragment : Fragment() {
    lateinit var repository: Repository

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        repository = (requireActivity() as MainActivity).repository

        viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO) {
            repository.insertHistory(History(null, "Hello", "World!"))
            repository.getHistoryList() // Query the database
            repository.deleteAllHistory() // Delete all records from the database

            val historyList: List<History> = repository.getHistoryList() // Query the database
            withContext(Dispatchers.Main) {
                // Process the list, update UI, etc.
            }
        }

    }
}

class Repository(private val database: AppDatabase) {
    suspend fun insertHistory(history: History) {
        database.historyDao().insert(history)
    }

    suspend fun getHistoryList(): List<History> {
        return database.historyDao().get()
    }

    suspend fun deleteAllHistory() {
        database.historyDao().delete()
    }
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    suspend fun delete()

    @Query("SELECT * FROM history")
    suspend fun get(): List<History>

    @Insert
    suspend fun insert(history: History)
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
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
    implementation("androidx.appcompat:appcompat:1.6.1") // Theme
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br><br><br>

---

## Coroutines by Application Architecture with Room
### Application
- GlobalScope.launch(Dispatchers.IO){}
- GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}

### AndroidViewModel(Not Recommended)
```
Activity, Fragment
  viewmodel = viewmodelfactory(uc)

AndroidViewModel
  db = room()
  repo = repository(db)
  uc = usecase(repo)
```

```
Activity, Fragment
  viewmodel = viewmodelfactory(uc)

AndroidViewModel
  db = room()
  repo = repository(db)
```

```
Activity, Fragment
  viewmodel = viewmodelfactory(uc)

AndroidViewModel
  db = room()
```


### ViewModel
- `viewModelScope.launch{}`
```
Activity, Fragment
  db = room()
  repo = repository(db)
  uc = usecase(repo)
  viewmodel = viewmodelfactory(uc)
```

```
Activity, Fragment
  db = room()
  repo = repository(db)
  viewmodel = viewmodelfactory(repo)
```

```
Activity, Fragment
  db = room()
  viewmodel = viewmodelfactory(db)
```
  
### Activity, Fragment
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`
```
Activity
  db = room()
  repo = repository(db)
```
```
Activity
  db = room()
```
  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`
```
Fragment
  db = room()
  repo = repository(db)
```
```
Fragment
  db = room()
```

### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`
```
Service
  db = room()
  repo = repository(db)
```
```
Service
  db = room()
```


### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`




