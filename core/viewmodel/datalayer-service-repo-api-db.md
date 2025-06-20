
## Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── java/com/example/myapplication/MainService.kt
│   │       ├── java/com/example/myapplication/MainRepository.kt
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
import androidx.activity.ComponentActivity
import java.io.Serializable

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Build
import android.widget.Toast
import androidx.annotation.RequiresApi
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    private val mainReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            if (intent?.action == "com.example.myapplication.RESULT") {
                val isSuccess = intent.getBooleanExtra("success", false)

                if (isSuccess) {
                    Toast.makeText(this@MainActivity, "데이터 로딩 완료", Toast.LENGTH_SHORT).show()
                } else {
                    Toast.makeText(this@MainActivity, "데이터 로딩 실패", Toast.LENGTH_SHORT).show()
                }
            }
        }
    }

    @RequiresApi(Build.VERSION_CODES.O)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val filter = IntentFilter().apply{ addAction("com.example.myapplication.RESULT") }
        registerReceiver(mainReceiver, filter, RECEIVER_NOT_EXPORTED)
        serviceForFetchDataFromServer(Item(mapOf("info" to 0), listOf()))
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(mainReceiver)
    }

    fun serviceForFetchDataFromServer(item: Item) {
        val intent = Intent(this, MainService::class.java)
        intent.putExtra("item", item)
        ContextCompat.startForegroundService(this, intent)
    }
}

data class Item(val information:Map<String, Any?>, val items:List<Any?>):
    Serializable {
    fun deepCopy(): Item {
        val copiedInfo = information.toMap()
        val copiedItems = items.toList()
        return Item(copiedInfo, copiedItems)
    }
}
```

`MainService.kt`
```kotlin
package com.example.myapplication

import android.app.Notification
import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.Service
import android.content.Intent
import android.os.Build
import android.os.IBinder
import android.util.Log
import androidx.core.app.NotificationCompat
import androidx.room.*
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.SupervisorJob
import kotlinx.coroutines.launch
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

class MainService : Service() {
    private lateinit var repository: DataRepository
    private val serviceScope = CoroutineScope(Dispatchers.IO + SupervisorJob())

    override fun onCreate() {
        super.onCreate()

        val db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
        val api = Retrofit.Builder().baseUrl("http://10.0.2.2:8000").addConverterFactory(GsonConverterFactory.create()).build().create(ApiService::class.java)
        repository = DataRepository(db.historyDao(), api)
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        startForeground(1, createNotification())

        val item = intent?.getSerializableExtra("item") as? Item

        item?.let {
            serviceScope.launch {
                try {
                    it.information?.get("info")

                    repository.insertHistory(History(null, "Hello", "World"))
                    val getResponse = repository.getRemote("Toy")
                    val postResponse = repository.postRemote(Request("Bob", 25))

                    Log.d("MainService", "API GET: $getResponse")
                    Log.d("MainService", "API POST: $postResponse")

                    sendResultBroadcast(true)
                } catch (e: Exception) {
                    Log.e("MainService", "Data fetch failed", e)
                    sendResultBroadcast(false)
                }
            }
        }

        sendResultBroadcast(success = true)
        return START_NOT_STICKY
    }

    private fun sendResultBroadcast(success: Boolean) {
        val intent = Intent("com.example.myapplication.RESULT").setPackage(packageName)
        intent.putExtra("success", success)
        sendBroadcast(intent)
    }

    private fun createNotification(): Notification {
        val channelId = "data_channel"
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                channelId,
                "data",
                NotificationManager.IMPORTANCE_LOW
            )
            val manager = getSystemService(NotificationManager::class.java)
            manager.createNotificationChannel(channel)
        }

        return NotificationCompat.Builder(this, channelId)
            .setContentTitle("Loading data")
            .setSmallIcon(R.drawable.ic_dialog_info)
            .build()
    }
}
```

`MainRepository.kt`
```kotlin
package com.example.myapplication

import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.RoomDatabase
import retrofit2.http.Body
import retrofit2.http.DELETE
import retrofit2.http.GET
import retrofit2.http.PATCH
import retrofit2.http.POST
import retrofit2.http.PUT
import retrofit2.http.Path

class DataRepository(
    private val dao: HistoryDao,
    private val api: ApiService
) {
    suspend fun insertHistory(history: History) = dao.insert(history)
    suspend fun getHistory(): List<History> = dao.get()
    suspend fun deleteHistory() = dao.delete()

    suspend fun getRemote(param: String) = api.getData(param)
    suspend fun postRemote(request: Request) = api.postData(request)
    suspend fun putRemote(param: String, request: Request) = api.putData(param, request)
    suspend fun patchRemote(param: String, fields: Map<String, @JvmSuppressWildcards Any>) = api.patchData(param, fields)
    suspend fun deleteRemote(param: String) = api.deleteData(param)
}

// Entity, DAO, DB
@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)

@Dao
interface HistoryDao {
    @Insert
    suspend fun insert(history: History)
    @Query("SELECT * FROM history") suspend fun get(): List<History>
    @Query("DELETE FROM history") suspend fun delete()
}

@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

// Retrofit interface
interface ApiService {
    @GET("/src/{param00}") suspend fun getData(@Path("param00") param00: String): Map<String, Any>
    @POST("/src") suspend fun postData(@Body param00: Request): Map<String, Any>
    @PUT("/src/{param00}") suspend fun putData(@Path("param00") param00: String, @Body param01: Request): Map<String, Any>
    @PATCH("/src/{param00}") suspend fun patchData(@Path("param00") param00: String, @Body param01: Map<String, @JvmSuppressWildcards Any>): Map<String, Any>
    @DELETE("/src/{param00}") suspend fun deleteData(@Path("param00") param00: String): Map<String, Any>
}

data class Request(val rqst00: String, val rqst01: Int)
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
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.myapplication">

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_DATA_SYNC" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication">

        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>

        <service
            android:name=".MainService"
            android:foregroundServiceType="dataSync"
            android:enabled="true"
            android:exported="false"
            tools:ignore="ForegroundServicePermission" />
    </application>

</manifest>
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
