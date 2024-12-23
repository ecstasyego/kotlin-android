
## Examples
### Example01: Basic
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                insertHistory(expression, result)
            }
            "query" -> {
                queryHistory()
            }
            "delete" -> {
                deleteHistory()
            }
        }
        return START_STICKY
    }

    // Insert history into the database synchronously
    private fun insertHistory(expression: String?, result: String?) {
        val history = History(expression = expression, result = result)
        db.historyDao().insert(history)
    }

    // Query all histories from the database synchronously
    private fun queryHistory() {
        val historyList = db.historyDao().get()
        // You can handle the result here, for example, send a broadcast or use LiveData
    }

    // Delete all histories from the database synchronously
    private fun deleteHistory() {
        db.historyDao().delete()
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br>


### Example02: ExecutorService
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase
import java.util.concurrent.Executors

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase
    private val executor = Executors.newSingleThreadExecutor()  // Thread pool for background work

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                executor.execute { insertHistory(expression, result) }
            }
            "query" -> {
                executor.execute { queryHistory() }
            }
            "delete" -> {
                executor.execute { deleteHistory() }
            }
        }
        return START_STICKY
    }

    // Insert history into the database
    private fun insertHistory(expression: String?, result: String?) {
        val history = History(expression = expression, result = result)
        db.historyDao().insert(history)
    }

    // Query all histories from the database
    private fun queryHistory() {
        val historyList = db.historyDao().get()
        // You can handle the result here
    }

    // Delete all histories from the database
    private fun deleteHistory() {
        db.historyDao().delete()
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br>

### Example03: Background Thread
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                insertHistory(expression, result)
            }
            "query" -> {
                queryHistory()
            }
            "delete" -> {
                deleteHistory()
            }
        }
        return START_STICKY
    }

    // Insert history into the database synchronously
    private fun insertHistory(expression: String?, result: String?) {
        val history = History(expression = expression, result = result)
        val thread = Thread{
            db.historyDao().insert(history)
        }
        thread.start()
        thread.join()
    }

    // Query all histories from the database synchronously
    private fun queryHistory() {
        // You can handle the result here, for example, send a broadcast or use LiveData
        val thread = Thread{
            val historyList = db.historyDao().get()
        }
        thread.start()
        thread.join()
    }

    // Delete all histories from the database synchronously
    private fun deleteHistory() {
        val thread = Thread{
            db.historyDao().delete()
        }
        thread.start()
        thread.join()
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br>


### Example04: Background Thread with BroadcastReceiver
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
import androidx.room.ColumnInfo
import androidx.room.Dao
import androidx.room.Database
import androidx.room.Entity
import androidx.room.Insert
import androidx.room.PrimaryKey
import androidx.room.Query
import androidx.room.Room
import androidx.room.RoomDatabase

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                insertHistory(expression, result)
            }
            "query" -> {
                queryHistory()
            }
            "delete" -> {
                deleteHistory()
            }
        }
        return START_STICKY
    }

    // Insert history into the database synchronously
    private fun insertHistory(expression: String?, result: String?) {
        val history = History(expression = expression, result = result)
        val thread = Thread{
            db.historyDao().insert(history)
        }
        thread.start()
        thread.join()
    }

    // Query all histories from the database synchronously
    private fun queryHistory() {
        // You can handle the result here, for example, send a broadcast or use LiveData
        val thread = Thread{
            val historyList = db.historyDao().get()
        }
        thread.start()
        thread.join()
    }

    // Delete all histories from the database synchronously
    private fun deleteHistory() {
        val thread = Thread{
            db.historyDao().delete()
        }
        thread.start()
        thread.join()
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```


<br>

### Example05: Coroutine
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
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
import kotlinx.coroutines.cancel
import kotlinx.coroutines.launch

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase
    private val serviceScope = CoroutineScope(Dispatchers.IO)  // Define the CoroutineScope

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                insertHistory(expression, result)
            }
            "query" -> {
                queryHistory()
            }
            "delete" -> {
                deleteHistory()
            }
        }
        return START_STICKY
    }

    // Insert history into the database
    private fun insertHistory(expression: String?, result: String?) {
        serviceScope.launch {
            val history = History(expression = expression, result = result)
            db.historyDao().insert(history)
        }
    }

    // Query all histories from the database
    private fun queryHistory() {
        serviceScope.launch {
            val historyList = db.historyDao().get()
            // You can handle the result here (e.g., send a broadcast or use LiveData)
        }
    }

    // Delete all histories from the database
    private fun deleteHistory() {
        serviceScope.launch {
            db.historyDao().delete()
        }
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    // Cancel the coroutine scope when the service is destroyed
    override fun onDestroy() {
        super.onDestroy()
        serviceScope.cancel()  // This cancels all ongoing coroutines when the service is destroyed
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```

<br>


### Example06: Coroutine with BroadcastReceiver
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import androidx.activity.ComponentActivity
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
import kotlinx.coroutines.cancel
import kotlinx.coroutines.launch

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Start the service to insert a record into the database
        val insertIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "insert")
            putExtra("expression", "Hello")
            putExtra("result", "World!")
        }
        startService(insertIntent)

        // Start the service to query the database
        val queryIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "query")
        }
        startService(queryIntent)

        // Start the service to delete all records from the database
        val deleteIntent = Intent(this, DatabaseService::class.java).apply {
            putExtra("action", "delete")
        }
        startService(deleteIntent)
    }
}


class DatabaseService : Service() {

    private lateinit var db: AppDatabase
    private val serviceScope = CoroutineScope(Dispatchers.IO)  // Define the CoroutineScope

    override fun onCreate() {
        super.onCreate()
        // Initialize the database here
        db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val action = intent?.getStringExtra("action")
        when (action) {
            "insert" -> {
                val expression = intent.getStringExtra("expression")
                val result = intent.getStringExtra("result")
                insertHistory(expression, result)
            }
            "query" -> {
                queryHistory()
            }
            "delete" -> {
                deleteHistory()
            }
        }
        return START_STICKY
    }

    // Insert history into the database
    private fun insertHistory(expression: String?, result: String?) {
        serviceScope.launch {
            val history = History(expression = expression, result = result)
            db.historyDao().insert(history)
        }
    }

    // Query all histories from the database
    private fun queryHistory() {
        serviceScope.launch {
            val historyList = db.historyDao().get()
            // You can handle the result here (e.g., send a broadcast or use LiveData)
        }
    }

    // Delete all histories from the database
    private fun deleteHistory() {
        serviceScope.launch {
            db.historyDao().delete()
        }
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    // Cancel the coroutine scope when the service is destroyed
    override fun onDestroy() {
        super.onDestroy()
        serviceScope.cancel()  // This cancels all ongoing coroutines when the service is destroyed
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
}

@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

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
    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```

