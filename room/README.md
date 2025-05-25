
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

### Database
```kotlin
@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}
```

### Repository
```kotlin
class HistoryRepository(private val database: AppDatabase) {
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
```

### UnitOfWork
```kotlin
class UnitOfWork(
    private val db: AppDatabase,
    val historyRepository: HistoryRepository
) {
    suspend fun <T> runInTransaction(block: suspend UnitOfWork.() -> T): T {
        return db.withTransaction {
            this.block()
        }
    }
}
```

### Usecase
```kotlin
class SaveHistoryUseCase(private val uow: UnitOfWork) {
    suspend fun execute() {
        uow.runInTransaction {
            historyRepository.deleteAll()
            historyRepository.insert(History(expression = "1 + 1", result = "2"))
        }
    }
}
```



### ViewModel
```kotlin
class HistoryViewModel(private val repository: HistoryRepository) : ViewModel() {
    private val _historyList = MutableLiveData<List<History>>()
    val historyList: LiveData<List<History>> get() = _historyList

    fun loadHistory() {
        viewModelScope.launch {
            val data = repository.getHistoryList()
            _historyList.postValue(data)
        }
    }

    fun addHistory(history: History) {
        viewModelScope.launch {
            repository.insertHistory(history)
            loadHistory() // Refresh the list after insertion
        }
    }

    fun clearHistory() {
        viewModelScope.launch {
            repository.deleteAllHistory()
            loadHistory() // Refresh the list after deletion
        }
    }
}
```

### ViewModelFactory
```kotlin
class HistoryViewModelFactory(private val database: AppDatabase) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(HistoryViewModel::class.java)) {
            return HistoryViewModel(HistoryRepository(database)) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```


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

