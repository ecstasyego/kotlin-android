
## Examples
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
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class MainActivity : ComponentActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build() // historyDB.sqlite, /data/data/<package_name>/databases/historyDB

        lifecycleScope.launch(Dispatchers.IO) {
            db.historyDao().insert(History(null, "Hello", "World!")) // Insert data into the database using coroutines
            db.historyDao().get() // Query the database
            db.historyDao().delete() // Delete all records from the database

            withContext(Dispatchers.Main) {

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

    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3")

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```
