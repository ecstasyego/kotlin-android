## Examples
### Example01: Coroutine
- viewModelScope.launch{}
  
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

import android.app.Application
import android.os.Bundle
import android.widget.LinearLayout
import androidx.activity.ComponentActivity
import androidx.lifecycle.AndroidViewModel
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
import kotlinx.coroutines.flow.MutableStateFlow
import kotlinx.coroutines.flow.StateFlow
import kotlinx.coroutines.launch

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase
    lateinit var viewModel: HistoryViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()
        viewModel = ViewModelProvider(this, HistoryViewModelFactory(application, db)).get(HistoryViewModel::class.java)

        lifecycleScope.launch {
            viewModel.historyList.collect { historyList ->
                // Update UI with historyList
            }
        }

        viewModel.addHistory(History(null, "Hello", "World!"))
        viewModel.loadHistory()
        viewModel.clearHistory()

    }
}

class HistoryViewModelFactory(private val application: Application, private val database: AppDatabase) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(HistoryViewModel::class.java)) {
            return HistoryViewModel(application, HistoryRepository(database)) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}

class HistoryViewModel(application: Application, private val repository: HistoryRepository) : AndroidViewModel(application) {
    private val _historyList = MutableStateFlow<List<History>>(emptyList())  // StateFlow로 변경
    val historyList: StateFlow<List<History>> get() = _historyList

    fun loadHistory() {
        viewModelScope.launch {
            val data = repository.getHistoryList()
            _historyList.value = data  // StateFlow에 값 할당
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
    implementation("androidx.activity:activity-ktx:1.7.2")  // Required for activityViewModels()
    implementation("androidx.fragment:fragment-ktx:1.6.1")  // Fragment KTX for enhanced features
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")  // ViewModel KTX
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.1")  // LiveData KTX

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


