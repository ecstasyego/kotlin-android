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
import android.widget.FrameLayout
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import androidx.fragment.app.DialogFragment
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
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
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    lateinit var frameLayout: FrameLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        frameLayout = FrameLayout(this).apply{ id = View.generateViewId()}
        setContentView(frameLayout)

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(frameLayout.id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    private lateinit var horizontalScrollView: HorizontalScrollView
    private lateinit var recyclerView:RecyclerView
    private lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Database
        val dbFile = requireContext().getDatabasePath("historyDB")
        if (dbFile.exists()) { dbFile.delete() }
        db = Room.databaseBuilder(
            requireContext(),
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        horizontalScrollView = HorizontalScrollView(requireContext())
        recyclerView = RecyclerView(requireContext())
        horizontalScrollView.addView(recyclerView)
        return horizontalScrollView
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // UI Update on background
        viewLifecycleOwner.lifecycleScope.launch {
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

                // UI
                withContext(Dispatchers.Main) {
                    display(daolist)
                }
            }
        }
    }

    private fun dataLoader(): List<Map<String, Any?>> {
        val upperCases: List<String> = listOf("A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z")
        val lowerCases: List<String> = upperCases.map{ it.lowercase() }
        return rowsIter(mutableMapOf<String, List<Any?>>(
            "C0" to (0 until 100).toList(),
            "C1" to List(100){ upperCases[Random.nextInt(0, 26)]},
            "C2" to List(100){ lowerCases[Random.nextInt(0, 26)]},
        ))
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

        recyclerView.layoutManager = GridLayoutManager(requireContext(), 4)
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


