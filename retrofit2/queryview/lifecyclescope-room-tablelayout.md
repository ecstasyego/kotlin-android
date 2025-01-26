

## Examples
### Usage: *.kt
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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import android.view.Gravity
import android.widget.HorizontalScrollView
import android.widget.LinearLayout
import android.widget.ScrollView
import android.widget.TableLayout
import android.widget.Toast
import android.view.View
import android.widget.TableRow
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.core.content.ContextCompat
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
    private lateinit var mainLayout: QueryView
    private lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainLayout = QueryView(this)
        setContentView(mainLayout)

        // Local
        val dbFile = applicationContext.getDatabasePath("historyDB")
        if (dbFile.exists()) { dbFile.delete() }
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()

        // Remote
        val retrofit = Retrofit.Builder()
            .baseUrl("https://raw.githubusercontent.com/")
            .addConverterFactory(ScalarsConverterFactory.create())
            .build()
        val apiService = retrofit.create(GithubApiService::class.java)

        val fileName = "005930.csv"
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
                                if (values.size >= 3) {
                                    val history = History(
                                        date = values[0],
                                        open = values[1].toDoubleOrNull(),
                                        high = values[2].toDoubleOrNull(),
                                        low = values[3].toDoubleOrNull(),
                                        close = values[4].toDoubleOrNull(),
                                    )
                                    csvDaoData.add(history)
                                }
                            }
                        }
                        Toast.makeText(applicationContext, "RETROFIT: SUCCESS", Toast.LENGTH_SHORT).show()

                        lifecycleScope.launch {
                            withContext(Dispatchers.IO) {
                                // [DATA] DAO DELETE
                                db.historyDao().delete()

                                // [DATA] DAO INSERTALL
                                db.historyDao().insertAll(csvDaoData)

                                // [DATA] DAO INSERT
                                csvDaoData.forEach {
                                    db.historyDao().insert(it)
                                }

                                // [DATA] DAO GET
                                val daolist = db.historyDao().get().reversed()

                                // UI
                                withContext(Dispatchers.Main) {
                                    display(daolist.slice(0 until 200))
                                }
                            }
                            Toast.makeText(applicationContext, "ROOM: SUCCESS", Toast.LENGTH_SHORT).show()
                        }

                    }
                } else {
                    Toast.makeText(applicationContext, "FAIL: ${response.message()}", Toast.LENGTH_SHORT).show()
                }
            }

            override fun onFailure(call: Call<ResponseBody>, t: Throwable) {
                Toast.makeText(applicationContext, "ERROR: ${t.message}", Toast.LENGTH_SHORT).show()
            }

            private fun display(data: List<History>){
                val rows = mutableListOf<TableRow>()
                for ((idx, dao) in (0 until data.size).zip(data)) {
                    if (idx==0){
                        rows.add(
                            TableRow(this@MainActivity).apply {
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "INDEX" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "RID" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "DATE" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "OPEN" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "HIGH" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "LOW" })
                                addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = "CLOSE" })
                            }
                        ) // columns
                    }
                    rows.add(
                        TableRow(this@MainActivity).apply {
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = idx.toString() }) // INDEX
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.rid.toString() }) // data
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.date.toString() }) // data
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.open.toString() }) // data
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.high.toString() }) // data
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.low.toString() }) // data
                            addView(TextView(this@MainActivity).apply { gravity = Gravity.CENTER; text = dao.close.toString() }) // data
                        }
                    )
                }
                rows.forEach { mainLayout.tableLayout.addView(it) }
            }
        })
    }
}

class QueryView @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0, defStyleRes: Int = 0) : LinearLayout(context, attrs, defStyleAttr, defStyleRes) {
    val tableLayout = TableLayout(context)
    val horizontalScrollView = HorizontalScrollView(context)
    val scrollView = ScrollView(context)

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        orientation = VERTICAL

        horizontalScrollView.apply{ id = View.generateViewId() }
        scrollView.apply{ id = View.generateViewId() }
        tableLayout.apply{
            id = View.generateViewId()
            layoutParams = TableLayout.LayoutParams(TableLayout.LayoutParams.MATCH_PARENT, TableLayout.LayoutParams.WRAP_CONTENT)
            setPadding(16, 16, 16, 16)
        }

        scrollView.addView(tableLayout)
        horizontalScrollView.addView(scrollView)
        addView(horizontalScrollView)
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

@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    suspend fun delete()

    @Query("SELECT * FROM history")
    suspend fun get(): List<History>

    @Insert
    suspend fun insert(history: History)

    @Insert
    suspend fun insertAll(histories: List<History>)
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val rid: Int = 0,
    @ColumnInfo(name = "date") val date: String?,
    @ColumnInfo(name = "open") val open: Double?,
    @ColumnInfo(name = "high") val high: Double?,
    @ColumnInfo(name = "low") val low: Double?,
    @ColumnInfo(name = "close") val close: Double?
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
