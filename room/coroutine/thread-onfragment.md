## Examples

### Example01: Thread
- Thread(Runnable{}).start()
   
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.View
import android.view.LayoutInflater
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.FrameLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : AppCompatActivity() {
    lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val frameLayout = FrameLayout(this@MainActivity).apply {id = View.generateViewId()}
        setContentView(frameLayout)

        db = Room.databaseBuilder(
            applicationContext,
            AppDatabase::class.java,
            "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
        ).build()

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(frameLayout.id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    private lateinit var db: AppDatabase

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        db = Room.databaseBuilder(
            requireContext().applicationContext,
            AppDatabase::class.java,
            "historyDB"
        ).build()
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        Thread(Runnable {
            db.historyDao().insert(History(null, "Hello", "World!"))
            db.historyDao().get()
            db.historyDao().delete()

            activity?.runOnUiThread {
                // Process the list, update UI, etc.
                Toast.makeText(requireContext(), "Hello, World!", Toast.LENGTH_SHORT).show()
            }
        }).start()

    }
}


@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}

@Dao
interface HistoryDao {
    @Query("DELETE FROM history")
    fun delete()

    @Query("SELECT * FROM history")
    fun get(): List<History>

    @Insert
    fun insert(history: History)
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

