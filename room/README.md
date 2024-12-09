
## Gradle
- https://developer.android.com/training/data-storage/room

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

## Entity: Table

## DAO: Query Interface

## Database

## Transaction

## Migrations

## LiveData

## Flow

## Room Test

