
## Gradle
- https://developer.android.com/training/data-storage/room

`APP-LEVEL`: Application/app/build.gralde.kts
```kts
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

## Device Explorer
- View > Tools Windows > Device Explorer
    - `Room`: `/data/data/[COM].[EXAMPLE].[APPLICATION]/databases`
        - Copy(Ctrl + Shift + S) > `C:\Users\[USER]\AppData\Local\Google\AndroidStudio[VERSION]\device-explorer\[EMULATOR]\_\data\data\[COM].[EXAMPLE].[APPLICATION]\databases\[DATABASE].db`


<br><br><br>

## Usage
```kotlin
// DELETE DATABASE
val dbFile = applicationContext.getDatabasePath("historyDB")
if (dbFile.exists()) {dbFile.delete()}

// BUILD
db = Room.databaseBuilder(
    applicationContext,
    AppDatabase::class.java,
    "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
).build()

// CLEAR TABLES
db.clearAllTables()


```

### Entity: Table
```kotlin
@Entity(tableName = "history")
data class History(
    @PrimaryKey(autoGenerate = true) val uid: Int? = null,
    @ColumnInfo(name = "expression") val expression: String?,
    @ColumnInfo(name = "result") val result: String?
)
```


### DAO: Query Interface
```kotlin
@Dao // DAO: Data Access Object
interface HistoryDao {
    @Query("DELETE FROM history")
    fun delete()

    @Query("SELECT * FROM history")
    fun get(): List<History>

    @Insert
    fun insert(history: History)
}
```

### Database
```kotlin
@Database(entities = [History::class], version = 1)
abstract class AppDatabase : RoomDatabase() {
    abstract fun historyDao(): HistoryDao
}
```



### Transaction
```kotlin
db = Room.databaseBuilder(applicationContext, AppDatabase::class.java, "historyDB").build() // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
db.runInTransaction {
    db.historyDao().insert(History(null, "Expression", "Result"))
    db.historyDao().delete()
}
```


### Migrations
```kotlin
```

### LiveData
```kotlin
```

### Flow
```kotlin
```

## Room Test
```kotlin
```

