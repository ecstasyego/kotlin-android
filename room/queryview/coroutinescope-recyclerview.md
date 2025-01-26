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

import android.app.Service
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.graphics.Rect
import android.os.Bundle
import android.os.IBinder
import android.view.Gravity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.HorizontalScrollView
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import androidx.fragment.app.DialogFragment
import androidx.lifecycle.lifecycleScope
import androidx.localbroadcastmanager.content.LocalBroadcastManager
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
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.SupervisorJob
import kotlinx.coroutines.cancel
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    private lateinit var horizontalScrollView: HorizontalScrollView
    private lateinit var recyclerView:RecyclerView
    private val broadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context, intent: Intent) {
            val daolist = intent.getSerializableExtra("data") as List<History>
            lifecycleScope.launch {
                display(daolist)
            }
        }

        private fun display(data: List<History>){
            val serializedData = mutableListOf<String>()
            for ( (idx, dao) in (0 until data.size).zip(data)){
                if (idx==0){
                    serializedData.add("INDEX")
                    serializedData.add("UID")
                    serializedData.add("EXPRESSION")
                    serializedData.add("RESULT")
                }
                serializedData.add(idx.toString())
                serializedData.add(dao.uid.toString())
                serializedData.add(dao.expression.toString())
                serializedData.add(dao.result.toString())
            }

            recyclerView.layoutManager = GridLayoutManager(this@MainActivity, 4)
            recyclerView.adapter = CustomAdapter(serializedData)
            recyclerView.addItemDecoration(GridSpacingItemDecoration(5))
        }

    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        horizontalScrollView = HorizontalScrollView(this)
        recyclerView = RecyclerView(this)
        horizontalScrollView.addView(recyclerView)
        setContentView(horizontalScrollView)

        val intent = Intent(this, DatabaseService::class.java)
        startService(intent)

        val filter = IntentFilter("com.example.myapplication.DATABASE_OPERATION")
        LocalBroadcastManager.getInstance(this).registerReceiver(broadcastReceiver, filter)
    }

    override fun onDestroy() {
        super.onDestroy()
        LocalBroadcastManager.getInstance(this).unregisterReceiver(broadcastReceiver)
    }
}

class DatabaseService : Service() {
    private lateinit var coroutineScope: CoroutineScope
    private lateinit var db: AppDatabase

    override fun onCreate() {
        super.onCreate()

        val dbFile = applicationContext.getDatabasePath("historyDB")
        if (dbFile.exists()) { dbFile.delete() }
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
        coroutineScope = CoroutineScope(Dispatchers.Main + SupervisorJob())
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        coroutineScope.launch {
            withContext(Dispatchers.IO) {
                val data = dataLoader()

                // [DATA] DAO DELETE
                db.historyDao().delete()

                // [DATA] DAO INSERTALL
                db.historyDao().insertAll(
                    data.map { row -> History(row["C0"] as Int, row["C1"] as String, row["C2"] as String) }.toList()
                )

                // [DATA] DAO INSERT
                data.forEach { row ->
                    db.historyDao().insert(
                        History(null, row["C1"] as String, row["C2"] as String)
                    )
                }

                // [DATA] DAO GET
                val daolist = db.historyDao().get().reversed()

                val intent = Intent("com.example.myapplication.DATABASE_OPERATION")
                intent.putExtra("data", ArrayList(daolist))
                LocalBroadcastManager.getInstance(applicationContext).sendBroadcast(intent)
            }
        }
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null // Not binding to any component
    }

    override fun onDestroy() {
        super.onDestroy()
        coroutineScope.cancel()
    }

    private fun dataLoader(): List<Map<String, Any?>> {
        val upperCases: List<String> = listOf("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")
        val lowerCases: List<String> = upperCases.map { it.lowercase() }
        return rowsIter(mutableMapOf<String, List<Any?>>(
            "C0" to (0 until 100).toList(),
            "C1" to List(100) { upperCases[Random.nextInt(0, 26)] },
            "C2" to List(100) { lowerCases[Random.nextInt(0, 26)] },
        ))
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
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)

fun rowsIter(dataMMap: MutableMap<String, List<Any?>>): List<Map<String, Any?>>{
    tailrec fun denestingPair( nestedRow: Any, unnestedRow: MutableList<Any?> = mutableListOf<Any?>() ): MutableList<Any?> {
        if (nestedRow is Pair<*, *>) {
            if (nestedRow.first is Pair<*, *>){
                unnestedRow.add(nestedRow.second)
                return denestingPair(nestedRow.first as Pair<*, *>, unnestedRow)
            } else {
                unnestedRow.add(nestedRow)
            }
        }
        return unnestedRow.reversed().map{it as Pair<String, Any?>}.toMutableList()
    }

    lateinit var nameColumn:List<String>
    lateinit var listPairs:List<Any?>
    for ( (idx, key) in (0 until dataMMap.keys.size).zip(dataMMap.keys) ) {
        nameColumn = List<String>(dataMMap[key]!!.size){ key as String }
        listPairs = if (idx == 0) nameColumn.zip(dataMMap[key]!!) else listPairs.zip(nameColumn.zip(dataMMap[key]!!))
    }
    return listPairs.map{denestingPair(it as Any).map{it as Pair<String, Any>}}.map{it.toMap()}
}
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.myapplication">

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
        <service android:name=".DatabaseService" android:enabled="true" android:exported="false"/>

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


