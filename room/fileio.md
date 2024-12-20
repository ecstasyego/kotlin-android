```kotlin
// 파일을 내부 저장소에 저장할 때
val fileOutputStream = openFileOutput("my_file.txt", Context.MODE_PRIVATE)
fileOutputStream.write("Hello, World!".toByteArray())
fileOutputStream.close()

// 내부 저장소에서 파일을 읽을 때
val fileInputStream = openFileInput("my_file.txt")
val content = fileInputStream.bufferedReader().use { it.readText() }
fileInputStream.close()
```
