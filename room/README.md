
## Gradle
- https://developer.android.com/training/data-storage/room

`APP-LEVEL`: Application/app/build.gralde.kts
```kts
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


```kts
android {
    buildTypes {
        getByName("debug") {
            isDebuggable = true
            applicationIdSuffix = ".debug"
            versionNameSuffix = "-debug"
        }
    }
}
```

## Device Explorer
- `adb pull /data/data/[COM].[EXAMPLE].[APPLICATION]/databases/[DATABASE].db .`
- View > Tools Windows > Device Explorer
    - `Room`: `/data/data/[COM].[EXAMPLE].[APPLICATION]/databases`
        - Copy(Ctrl + Shift + S) > `C:\Users\[USER]\AppData\Local\Google\AndroidStudio[VERSION]\device-explorer\[EMULATOR]\_\data\data\[COM].[EXAMPLE].[APPLICATION]\databases\[DATABASE].db`

## Sqlite3
### Usage on desktop
- CLI
    - sqlite-tools-win-x64-*.zip: https://www.sqlite.org/download.html
- GUI
    - DB Browser for SQLite - Standard installer for 64-bit Windows: https://sqlitebrowser.org/dl/

### Usage on mobile
```kotlin
// DELETE DATABASE
val dbFile = applicationContext.getDatabasePath("historyDB")
if (dbFile.exists()) {dbFile.delete()}

// BUILD
db = Room.databaseBuilder(
    applicationContext,
    AppDatabase::class.java,
    "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
).build()

// CLEAR TABLES
db.clearAllTables()
```


<br><br><br>

## Core
- ViewModelFactory: Dependency Injection
  - ViewModel: View
    - UseCase: Business Logic
      - UnitOfWork(High-Level SQL Interface): Transaction
        - Repository(Middle-Level SQL Interface): API(Retrofit2)
          - Database(Room)
            - DAO(Low-Level SQL Interface) 
              - Table(Entity): ORM
```kotlin
db = Database()
repo = Repository(db)
uow = UnitOfWork(repo)
uc = UseCase(uow)
vm = ViewModel(uc)
ViewModelFactory(db)
```


                  
### Entity: Table
```kotlin
@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```


### DAO: Query Interface
```kotlin
@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    suspend fun delete()

    @Query("SELECT * FROM history")
    suspend fun get(): List<History>

    @Insert
    suspend fun insert(history: History)
}
```
```kotlin
db.historyDao().delete()
db.historyDao().get()
db.historyDao().insert()
```

### Database
```kotlin
@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}
```
```kotlin
db = Room.databaseBuilder(
    applicationContext,
    AppDatabase::class.java,
    "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
).build()
```

### Repository
```kotlin
class HistoryRepository(private val historyDao: HistoryDao) {
    suspend fun insertHistory(history: History) {
        historyDao.insert(history)
    }

    suspend fun getHistoryList(): List<History> {
        return historyDao.get()
    }

    suspend fun deleteAllHistory() {
        historyDao.delete()
    }
}
```
```kotlin
repository = HistoryRepository(db.historyDao())
repository.insertHistory()
repository.getHistoryList()
repository.deleteAllHistory()
```

### UnitOfWork
```kotlin
class UnitOfWork(private val db: AppDatabase) {
    val historyRepository = HistoryRepository(db.historyDao())

    suspend fun <T> runInTransaction(block: suspend UnitOfWork.() -> T): T {
        return db.withTransaction {
            this@UnitOfWork.block()
        }
    }
}
```
```kotlin
uow.runInTransaction{
}
```

### Usecase
```kotlin
class SaveHistoryUseCase(private val uow: UnitOfWork) {
    suspend fun execute(): List<History> {
        return uow.runInTransaction {
            historyRepository.deleteAll()
            historyRepository.insert(History(expression = "1 + 1", result = "2"))
            historyRepository.getHistoryList()
        }
    }
}
```
```kotlin
saveHistoryUseCase = SaveHistoryUseCase(UnitOfWork(db))
saveHistoryUseCase.execute()
```



### ViewModel & ViewModelFactory
```kotlin
class HistoryViewModel(private val saveHistoryUseCase: SaveHistoryUseCase) : ViewModel() {
    private val _historyList = MutableLiveData<List<History>>()
    val historyList: LiveData<List<History>> get() = _historyList

    fun loadHistory() {
        viewModelScope.launch {
            val updatedList = saveHistoryUseCase.execute()
            _historyList.postValue(updatedList)
        }
    }
}

class HistoryViewModelFactory(private val database: AppDatabase) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(HistoryViewModel::class.java)) {
            return HistoryViewModel(SaveHistoryUseCase(UnitOfWork(database))) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```
```kotlin
val viewModelFactory = HistoryViewModelFactory(db)

viewModel = ViewModelProvider(this, viewModelFactory).get(HistoryViewModel::class.java)
viewModel.historyList.observe(this) { historyList ->
    for (item in historyList) {
        Log.d("HistoryItem", "${item.expression} = ${item.result}")
    }
}
viewModel.loadHistory()
```


## with Presentation Layer
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.util.Log
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider
import androidx.lifecycle.lifecycleScope
import androidx.lifecycle.viewModelScope
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

class MainActivity : AppCompatActivity() {
    lateinit var db: AppDatabase
    lateinit var repository: HistoryRepository

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply {
            addView( FrameLayout(this@MainActivity).apply {id = View.generateViewId()} )
        }
        setContentView(main_layout)

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()
        val viewModelFactory = HistoryViewModelFactory(db)
        val viewModel = ViewModelProvider(this, viewModelFactory).get(HistoryViewModel::class.java)
        viewModel.historyList.observe(this) { historyList ->
            for (item in historyList) {
                Log.d("HistoryItem", "${item.expression} = ${item.result}")
            }
        }
        viewModel.loadHistory()

        lifecycleScope.launch(Dispatchers.IO) {

            repository = HistoryRepository(db.historyDao())

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
    lateinit var repository: HistoryRepository

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



class HistoryViewModelFactory(private val database: AppDatabase) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(HistoryViewModel::class.java)) {
            val unitOfWork = UnitOfWork(database)
            val useCase = SaveHistoryUseCase(unitOfWork)
            return HistoryViewModel(useCase) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}



class HistoryViewModel(private val saveHistoryUseCase: SaveHistoryUseCase) : ViewModel() {
    private val _historyList = MutableLiveData<List<History>>()
    val historyList: LiveData<List<History>> get() = _historyList

    fun loadHistory() {
        viewModelScope.launch {
            val updatedList = saveHistoryUseCase.execute()
            _historyList.postValue(updatedList)
        }
    }
}


class SaveHistoryUseCase(private val uow: UnitOfWork) {
    suspend fun execute(): List<History> {
        return uow.runInTransaction {
            historyRepository.deleteAllHistory() // <- 수정됨
            historyRepository.insertHistory(History(expression = "1 + 1", result = "2"))
            historyRepository.getHistoryList()
        }
    }
}



class UnitOfWork(private val db: AppDatabase) {
    val historyRepository = HistoryRepository(db.historyDao())

    suspend fun <T> runInTransaction(block: suspend UnitOfWork.() -> T): T {
        return db.withTransaction {
            this@UnitOfWork.block()
        }
    }
}


class HistoryRepository(private val historyDao: HistoryDao) {
    suspend fun insertHistory(history: History) {
        historyDao.insert(history)
    }

    suspend fun getHistoryList(): List<History> {
        return historyDao.get()
    }

    suspend fun deleteAllHistory() {
        historyDao.delete()
    }
}



@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}


@Dao
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

<br><br><br>

---

## Management

### Transaction
```kotlin
db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build() // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
db.runInTransaction {
    db.historyDao().insert(History(null, "Expression", "Result"))
    db.historyDao().delete()
}
```


### Migrations
```kotlin
```

### LiveData
```kotlin
```

### Flow
```kotlin
```

## Room Test
```kotlin
```

