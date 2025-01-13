
## Examples
### Example01: Retrofit2 and Room on Service
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/activity_splash.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.app.Service
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Build
import android.os.Bundle
import android.os.IBinder
import android.view.View
import android.widget.LinearLayout
import android.widget.ProgressBar
import android.widget.TextView
import android.widget.Toast
import androidx.activity.result.ActivityResultLauncher
import androidx.activity.result.contract.ActivityResultContracts
import androidx.annotation.RequiresApi
import androidx.appcompat.app.AppCompatActivity
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
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.launch
import okhttp3.ResponseBody
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Retrofit
import retrofit2.Response
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.InputStreamReader

class SplashActivity : AppCompatActivity() {

    private lateinit var progressBar: ProgressBar
    private lateinit var progressText: TextView
    private lateinit var timeText: TextView
    private lateinit var db: AppDatabase
    private lateinit var csvDownloadReceiver: BroadcastReceiver

    private val launchMainActivity: ActivityResultLauncher<Intent> = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()) {
        // Handle the result if needed
    }

    @RequiresApi(Build.VERSION_CODES.O)
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_splash)

        // Initialize views
        progressBar = findViewById(R.id.progressBar)
        progressText = findViewById(R.id.progressText)
        timeText = findViewById(R.id.timeText)

        // Set up Room Database
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB")
            .build()

        // Register broadcast receiver to get download status
        val filter = IntentFilter("com.example.myapplication.CSV_DOWNLOAD_COMPLETED")
        csvDownloadReceiver = object : BroadcastReceiver() {
            override fun onReceive(context: Context?, intent: Intent?) {
                val success = intent?.getBooleanExtra("success", false) ?: false
                val code = intent?.getStringExtra("code") ?: ""
                val totalFiles = intent?.getIntExtra("totalFiles", 0) ?: 0
                val completedFiles = intent?.getIntExtra("completedFiles", 0) ?: 0
                val elapsedTime = intent?.getLongExtra("elapsedTime", 0L) ?: 0L

                // Update the progress bar and display remaining time
                val progress = (completedFiles * 100) / totalFiles
                progressBar.progress = progress
                progressText.text = "$progress%"

                val estimatedTime = if (completedFiles > 0) {
                    val avgTimePerFile = elapsedTime / completedFiles
                    val remainingFiles = totalFiles - completedFiles
                    avgTimePerFile * remainingFiles
                } else {
                    0L
                }

                val remainingTimeFormatted = formatTime(estimatedTime)
                timeText.text = "TIME: $remainingTimeFormatted"

                // Transition to MainActivity when all files are downloaded
                if (completedFiles == totalFiles) {
                    if (success) {
                        Toast.makeText(applicationContext, "Data($code) saved to Room successfully!", Toast.LENGTH_SHORT).show()
                    } else {
                        Toast.makeText(applicationContext, "Error saving data.", Toast.LENGTH_SHORT).show()
                    }

                    // Start MainActivity after download completion
                    val intent = Intent(applicationContext, MainActivity::class.java)
                    launchMainActivity.launch(intent)
                    finish()
                }
            }
        }
        registerReceiver(csvDownloadReceiver, filter, RECEIVER_NOT_EXPORTED)

        // Start the service to download CSV data
        val stocks = arrayOf("005930.csv", "000660.csv")
        val intent = Intent(this, RemoteService::class.java)
        intent.putExtra("fileNames", stocks)
        startService(intent)

        // Show the progress bar when downloading starts
        progressBar.visibility = View.VISIBLE
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(csvDownloadReceiver)
    }

    // Helper function to format time in HH:mm:ss format
    private fun formatTime(milliseconds: Long): String {
        val hours = (milliseconds / 3600000).toInt()
        val minutes = (milliseconds % 3600000 / 60000).toInt()
        val seconds = (milliseconds % 60000 / 1000).toInt()
        return String.format("%02d:%02d:%02d", hours, minutes, seconds)
    }
}


class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this).apply{addView(TextView(this@MainActivity).apply{text="Main Activity"})})
    }
}

class RemoteService : Service() {
    private lateinit var db: AppDatabase
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://raw.githubusercontent.com/")
        .addConverterFactory(ScalarsConverterFactory.create())
        .build()
    private val apiService = retrofit.create(GithubApiService::class.java)

    private var totalFiles = 0
    private var completedFiles = 0
    private var startTime: Long = 0

    override fun onCreate() {
        super.onCreate()
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB")
            .build()
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val fileNames = intent?.getStringArrayExtra("fileNames") ?: arrayOf("default.csv")
        totalFiles = fileNames.size // 총 파일 수
        startTime = System.currentTimeMillis() // 시작 시간 기록
        for (fileName in fileNames) {
            csvDownload(fileName)
        }
        return START_NOT_STICKY
    }

    private fun csvDownload(fileName: String) {
        val code = fileName.split(",")[0]
        val call = apiService.downloadCsvFile(fileName)
        call.enqueue(object : Callback<ResponseBody> {
            override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
                if (response.isSuccessful) {
                    val body = response.body()
                    body?.let {
                        val reader = BufferedReader(InputStreamReader(it.byteStream()))
                        val csvDaoData = mutableListOf<History>()

                        reader.useLines { lines ->
                            lines.forEach { line ->
                                val values = line.split(",").map { it.trim() }
                                if (values.size >= 5) {
                                    val history = History(
                                        date = values[0],
                                        open = values[1].toDoubleOrNull(),
                                        high = values[2].toDoubleOrNull(),
                                        low = values[3].toDoubleOrNull(),
                                        close = values[4].toDoubleOrNull(),
                                        code = code
                                    )
                                    csvDaoData.add(history)
                                }
                            }
                        }

                        // Save data to Room database on background thread
                        GlobalScope.launch(Dispatchers.IO) {
                            db.historyDao().delete()  // Clear existing data
                            csvDaoData.forEach {
                                db.historyDao().insert(it)  // Insert new data
                            }

                            // Track elapsed time
                            val elapsedTime = System.currentTimeMillis() - startTime
                            completedFiles++

                            val broadcastIntent =
                                Intent("com.example.myapplication.CSV_DOWNLOAD_COMPLETED").setPackage(packageName)
                            broadcastIntent.putExtra("success", true)
                            broadcastIntent.putExtra("code", code)
                            broadcastIntent.putExtra("totalFiles", totalFiles)
                            broadcastIntent.putExtra("completedFiles", completedFiles)
                            broadcastIntent.putExtra("elapsedTime", elapsedTime)
                            sendBroadcast(broadcastIntent)
                        }
                    }
                } else {
                    val broadcastIntent =
                        Intent("com.example.myapplication.CSV_DOWNLOAD_COMPLETED").setPackage(packageName)
                    broadcastIntent.putExtra("success", false)
                    sendBroadcast(broadcastIntent)
                }
            }

            override fun onFailure(call: Call<ResponseBody>, t: Throwable) {
                val broadcastIntent =
                    Intent("com.example.myapplication.CSV_DOWNLOAD_COMPLETED").setPackage(packageName)
                broadcastIntent.putExtra("success", false)
                sendBroadcast(broadcastIntent)
            }
        })
    }
}


interface GithubApiService {
    @GET("ecstasyego/CSV/main/{fileName}")
    fun downloadCsvFile(@Path("fileName") fileName: String): Call<ResponseBody>
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
    @PrimaryKey(autoGenerate = true) val rid: Int = 0,
    @ColumnInfo(name = "date") val date: String?,
    @ColumnInfo(name = "open") val open: Double?,
    @ColumnInfo(name = "high") val high: Double?,
    @ColumnInfo(name = "low") val low: Double?,
    @ColumnInfo(name = "close") val close: Double?,
    @ColumnInfo(name = "code") val code: String?
)
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">

        <activity
            android:name=".SplashActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity android:name=".MainActivity"/>

        <service android:name=".RemoteService" android:enabled="true" android:exported="false"/>

    </application>
</manifest>
```

`activity_splash.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyleHorizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:max="100"
        android:progress="0" />

    <TextView
        android:id="@+id/progressText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="0%"
        android:textSize="16sp" />

    <TextView
        android:id="@+id/timeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="8dp"
        android:text="TIME: "
        android:textSize="16sp" />

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
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor

    implementation("com.squareup.retrofit2:retrofit:2.9.0") // Retrofit
    implementation("com.squareup.retrofit2:converter-gson:2.9.0") // Retrofit GSON
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0") // Retrofit SCALARS
    implementation("com.squareup.okhttp3:okhttp:4.9.3")
}
```

