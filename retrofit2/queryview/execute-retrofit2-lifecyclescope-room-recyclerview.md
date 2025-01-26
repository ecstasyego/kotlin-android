
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

import android.graphics.Rect
import android.os.Bundle
import android.view.Gravity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.HorizontalScrollView
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import androidx.fragment.app.DialogFragment
import androidx.lifecycle.lifecycleScope
import androidx.recyclerview.widget.GridLayoutManager
import androidx.recyclerview.widget.RecyclerView
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
import retrofit2.Retrofit
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.InputStreamReader

class MainActivity : AppCompatActivity() {
    private lateinit var horizontalScrollView: HorizontalScrollView
    private lateinit var recyclerView:RecyclerView
    private lateinit var db: AppDatabase
    private lateinit var retrofit: Retrofit

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        horizontalScrollView = HorizontalScrollView(this)
        recyclerView = RecyclerView(this)
        horizontalScrollView.addView(recyclerView)
        setContentView(horizontalScrollView)

        // Database
        val dbFile = applicationContext.getDatabasePath("historyDB")
        if (dbFile.exists()) {dbFile.delete()}
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
        retrofit = Retrofit.Builder().baseUrl("https://raw.githubusercontent.com/").addConverterFactory(ScalarsConverterFactory.create()).build()

        lifecycleScope.launch(Dispatchers.IO) {
            // [DATA] DAO GET
            dataLoader("005930.csv")
            val daolist = db.historyDao().get().reversed()

            // UI
            withContext(Dispatchers.Main) {
                display(daolist)
                Toast.makeText(applicationContext, "ROOM: SUCCESS", Toast.LENGTH_SHORT).show()
            }
        }
    }

    private fun dataLoader(fileName: String){
        val apiService = retrofit.create(GithubApiService::class.java)
        val call = apiService.downloadCsvFile(fileName)
        val response = call.execute()

        try {
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

                    // [DATA] DAO DELETE
                    db.historyDao().delete()

                    // [DATA] DAO INSERTALL
                    db.historyDao().insertAll(csvDaoData)

                    // [DATA] DAO INSERT
                    csvDaoData.forEach {
                        db.historyDao().insert(it)
                    }

                }
            } else {
                lifecycleScope.launch(Dispatchers.Main) {
                    Toast.makeText(applicationContext, "FAIL: ${response.message()}", Toast.LENGTH_SHORT).show()
                }
            }
        } catch (e: Exception) {
            lifecycleScope.launch(Dispatchers.Main) {
                Toast.makeText(applicationContext, "Error: ${e.message}", Toast.LENGTH_SHORT).show()
            }
        }
    }

    private fun display(data: List<History>){
        val serializedData = mutableListOf<String>()
        for ( (idx, dao) in (0 until data.size).zip(data)){
            if (idx==0){
                serializedData.add("INDEX")
                serializedData.add("RID")
                serializedData.add("DATE")
                serializedData.add("OPEN")
                serializedData.add("HIGH")
                serializedData.add("LOW")
                serializedData.add("CLOSE")
            }
            serializedData.add(idx.toString())
            serializedData.add(dao.rid.toString())
            serializedData.add(dao.date.toString())
            serializedData.add(dao.open.toString())
            serializedData.add(dao.high.toString())
            serializedData.add(dao.low.toString())
            serializedData.add(dao.close.toString())
        }

        recyclerView.layoutManager = GridLayoutManager(this@MainActivity, 7)
        recyclerView.adapter = CustomAdapter(serializedData)
        recyclerView.addItemDecoration(GridSpacingItemDecoration(5))
    }
}

class CustomAdapter(private val items: MutableList<String>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
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
            gravity = Gravity.CENTER
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

class GridSpacingItemDecoration(private val spacing: Int) : RecyclerView.ItemDecoration() {
    override fun getItemOffsets(outRect: Rect, view: View, parent: RecyclerView, state: RecyclerView.State) {
        super.getItemOffsets(outRect, view, parent, state)

        outRect.left = spacing
        outRect.right = spacing
        outRect.top = spacing
        outRect.bottom = spacing
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
    fun delete()

    @Query("SELECT * FROM history")
    fun get(): List<History>

    @Insert
    fun insert(history: History)

    @Insert
    fun insertAll(histories: List<History>)
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
