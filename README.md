# kotlin-android
`MainActivity.kt`
```kotlin
package com.example.myapplication
import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```
`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!" />
</LinearLayout>
```


## API Reference
- https://developer.android.com/reference
- https://developer.android.com/reference/classes
- https://developer.android.com/reference/packages


## Application Architecture
- https://developer.android.com/topic/architecture

### Activity
```kotlin
onCreate()
  onStart()
    onResume()
    onPause()
  onStop()
onDestroy()
```

### Fragment

### Intent

### Service

### Broadcast Receiver

### Content Provider

### View

### Context

### RecyclerView

### SharedPreferences

### Room Database


