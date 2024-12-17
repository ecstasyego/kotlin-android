
## Gradle
- https://developer.android.com/training/data-storage/room

`APP-LEVEL`: Application/app/build.gralde.kts
```kts
dependencies {
    val room_version = "2.6.1"
    implementation("androidx.room:room-runtime:$room_version")
    annotationProcessor("androidx.room:room-compiler:$room_version") // If this project only uses Java source, use the Java annotationProcessor, No additional plugins are necessary
    implementation("androidx.room:room-ktx:$room_version") // optional - Kotlin Extensions and Coroutines support for Room
    implementation("androidx.room:room-rxjava2:$room_version") // optional - RxJava2 support for Room
    implementation("androidx.room:room-rxjava3:$room_version") // optional - RxJava3 support for Room
    implementation("androidx.room:room-guava:$room_version") // optional - Guava support for Room, including Optional and ListenableFuture
    testImplementation("androidx.room:room-testing:$room_version") // optional - Test helpers
    implementation("androidx.room:room-paging:$room_version") // optional - Paging 3 Integration
}
```


<br><br><br>

## Usage
```kotlin
db = Room.databaseBuilder(
    applicationContext,
    AppDatabase::class.java,
    "historyDB" // historyDB.sqlite, /data/data/<package_name>/databases/historyDB
).build()
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

