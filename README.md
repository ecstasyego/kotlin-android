# kotlin-android

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


