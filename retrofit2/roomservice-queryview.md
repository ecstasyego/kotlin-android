## Examples
### Example01: Retrofit2 and Room on Service + RecyclerView
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

import android.annotation.SuppressLint
import android.os.Bundle
import android.widget.Toast
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import android.content.Intent
import android.content.BroadcastReceiver
import android.content.Context
import android.content.IntentFilter
import android.app.Service
import android.graphics.Rect
import android.os.IBinder
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import androidx.fragment.app.DialogFragment
import androidx.lifecycle.lifecycleScope
import androidx.recyclerview.widget.GridLayoutManager
import androidx.recyclerview.widget.RecyclerView
import kotlinx.coroutines.GlobalScope
import androidx.room.Room
import androidx.room.Database
import androidx.room.RoomDatabase
import androidx.room.Dao
import androidx.room.Insert
import androidx.room.Query
import androidx.room.ColumnInfo
import androidx.room.Entity
import androidx.room.PrimaryKey
import okhttp3.ResponseBody
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.InputStreamReader

class MainActivity : AppCompatActivity() {
    private lateinit var db: AppDatabase
    private lateinit var recyclerView: RecyclerView
    private lateinit var adapter: CsvAdapter
    private lateinit var serializedData: MutableList<String>

    private val csvDownloadReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val success = intent?.getBooleanExtra("success", false) ?: false
            if (success) {
                val code = intent?.getStringExtra("code") ?: ""
                Toast.makeText(applicationContext, "Data($code) saved to Room successfully!", Toast.LENGTH_SHORT).show()
                lifecycleScope.launch {
                    serializedData.clear()
                    for (historyDao in db.historyDao().get()){
                        serializedData.add(historyDao.date.toString())
                        serializedData.add(historyDao.open.toString())
                        serializedData.add(historyDao.high.toString())
                        serializedData.add(historyDao.low.toString())
                        serializedData.add(historyDao.close.toString())
                        serializedData.add(historyDao.code.toString())
                    }
                    adapter.notifyDataSetChanged()
                }

            } else {
                Toast.makeText(applicationContext, "Error saving data.", Toast.LENGTH_SHORT).show()
            }
        }
    }

    @SuppressLint("NewApi")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        serializedData = mutableListOf<String>()
        adapter = CsvAdapter(serializedData)
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = GridLayoutManager(this, 6)
        recyclerView.adapter = adapter
        recyclerView.addItemDecoration(GridSpacingItemDecoration(10))
        setContentView(recyclerView)

        val dbFile = applicationContext.getDatabasePath("historyDB")
        if (dbFile.exists()) { dbFile.delete() }
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB")
            .build()

        val filter = IntentFilter("com.example.myapplication.CSV_DOWNLOAD_COMPLETED")
        registerReceiver(csvDownloadReceiver, filter, RECEIVER_NOT_EXPORTED)

        // Start the service to download CSV data
        val stocks = arrayOf("005930.csv", "000660.csv")
        val intent = Intent(this, RemoteService::class.java)
        intent.putExtra("fileNames", stocks)
        startService(intent)
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(csvDownloadReceiver)
    }
}

class GridSpacingItemDecoration(private val spacing: Int) : RecyclerView.ItemDecoration() {
    override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {
        super.getItemOffsets(outRect, view, parent, state)

        outRect.left = spacing
        outRect.right = spacing
        outRect.top = spacing
        outRect.bottom = spacing
    }
}

class CsvAdapter(private val items: List<String>) : RecyclerView.Adapter<CsvAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val cardView: CardView = itemView as CardView
        val textView: TextView = cardView.findViewById(cardView.getChildAt(0).id)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val cardView = CardView(parent.context).apply {
            layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
            radius = 8f // set corner radius
            elevation = 4f // set elevation for shadow
            setContentPadding(16, 16, 16, 16) // add padding inside CardView
        }

        val textView = TextView(parent.context).apply{
            id = View.generateViewId()
            layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
            maxLines = 1 // one line option
            ellipsize = android.text.TextUtils.TruncateAt.END // one line option
            setSingleLine(true) // one line option
            setPadding(16, 16, 16, 16)
        }
        cardView.addView(textView)
        cardView.setOnClickListener {
            TextFragment().apply {
                arguments = Bundle().apply { putString("text", textView.text.toString()) }
                show((parent.context as MainActivity).supportFragmentManager.beginTransaction(), "dialog")
            }

        }

        return ViewHolder(cardView)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position]
    }

    override fun getItemCount(): Int = items.size
}


class TextFragment : DialogFragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        var cellText = arguments?.getString("text", "Default Text")
        return TextView(requireContext()).apply {text = cellText }
    }
}


class RemoteService : Service() {
    private lateinit var db: AppDatabase
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://raw.githubusercontent.com/")
        .addConverterFactory(ScalarsConverterFactory.create())
        .build()
    private val apiService = retrofit.create(GithubApiService::class.java)

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

                            // Send broadcast to inform that data has been saved
                            val broadcastIntent =
                                Intent("com.example.myapplication.CSV_DOWNLOAD_COMPLETED").setPackage(packageName)
                            broadcastIntent.putExtra("success", true)
                            broadcastIntent.putExtra("code", code)
                            sendBroadcast(broadcastIntent)
                        }
                    }
                } else {
                    // Broadcast an error if the download fails
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
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <service android:name=".RemoteService" android:enabled="true" android:exported="false"/>

    </application>
</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

`history_item.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="16dp">

    <TextView
        android:id="@+id/dateTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Date"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/openTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Open"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/highTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="High"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/lowTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Low"
        android:layout_weight="1"/>

    <TextView
        android:id="@+id/closeTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="Close"
        android:layout_weight="1"/>
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
