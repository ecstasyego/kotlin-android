
## Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── java/com/example/myapplication/MainService.kt
│   │       ├── res/drawable/ic_dialog_info.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.util.Log
import android.widget.LinearLayout
import androidx.activity.ComponentActivity
import androidx.lifecycle.lifecycleScope
import androidx.room.*
import androidx.room.Query
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import retrofit2.http.*

class MainActivity : ComponentActivity() {
    private lateinit var repository: DataRepository

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        val db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
        val retrofit = Retrofit.Builder().baseUrl("http://10.0.2.2:8000").addConverterFactory(GsonConverterFactory.create()).build()
        val api = retrofit.create(ApiService::class.java)
        repository = DataRepository(db.historyDao(), api)

        lifecycleScope.launch {
            withContext(Dispatchers.IO) {
                try {
                    // Room
                    repository.insertHistory(History(null, "Hello", "World!"))
                    val localData = repository.getHistory()
                    Log.d("ROOM", "History size: ${localData.size}")

                    // Retrofit
                    val getResponse = repository.getRemote("Toy")
                    Log.d("API_GET", getResponse.toString())

                    val postResponse = repository.postRemote(Request("Bob", 25))
                    Log.d("API_POST", postResponse.toString())
                } catch (e: Exception) {
                    Log.e("APP_ERROR", "Error occurred", e)
                }
            }
        }
    }
}

class DataRepository(
    private val dao: HistoryDao,
    private val api: ApiService
) {
    // Room
    suspend fun insertHistory(history: History) = dao.insert(history)
    suspend fun getHistory(): List<History> = dao.get()
    suspend fun deleteHistory() = dao.delete()

    // Retrofit
    suspend fun getRemote(param: String) = api.getData(param)
    suspend fun postRemote(request: Request) = api.postData(request)
    suspend fun putRemote(param: String, request: Request) = api.putData(param, request)
    suspend fun patchRemote(param: String, fields: Map<String, @JvmSuppressWildcards Any>) = api.patchData(param, fields)
    suspend fun deleteRemote(param: String) = api.deleteData(param)
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao
interface HistoryDao {
    @Insert suspend fun insert(history: History)
    @Query("SELECT * FROM history") suspend fun get(): List<History>
    @Query("DELETE FROM history") suspend fun delete()
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)

interface ApiService {
    @GET("/src/{param00}")
    suspend fun getData(@Path("param00") param00: String): Map<String, Any>

    @POST("/src")
    suspend fun postData(@Body param00: Request): Map<String, Any>

    @PUT("/src/{param00}")
    suspend fun putData(@Path("param00") param00: String, @Body param01: Request): Map<String, Any>

    @PATCH("/src/{param00}")
    suspend fun patchData(@Path("param00") param00: String, @Body param01: Map<String, @JvmSuppressWildcards Any>): Map<String, Any>

    @DELETE("/src/{param00}")
    suspend fun deleteData(@Path("param00") param00: String): Map<String, Any>
}

data class Request(
    val rqst00: String,
    val rqst01: Int
)
```

`MainService.kt`
```kotlin
```

`ic_dialog_info.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp"
    android:height="24dp"
    android:viewportWidth="24"
    android:viewportHeight="24">

    <path
        android:fillColor="#FF000000"
        android:pathData="M11,7h2v2h-2zM12,2C6.48,2 2,6.48 2,12s4.48,10 10,10 10,-4.48 10,-10S17.52,2 12,2zM13,17h-2v-6h2v6z"/>
</vector>
```

`AndroidManifest.xml`
```xml
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
