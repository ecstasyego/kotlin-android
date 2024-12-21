## PressTimer
### Example01: Handler
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

import android.annotation.SuppressLint
import android.os.Bundle
import android.view.MotionEvent
import android.widget.ProgressBar
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity() {
    private lateinit var progressBar: ProgressBar
    private lateinit var elapsedTimeText: TextView
    private var startTime: Long = 0
    private var isPressed = false
    private var elapsedTime: Long = 0
    private var job: Job? = null  // Job for controlling the coroutine

    @SuppressLint("ClickableViewAccessibility")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Initialize views
        progressBar = findViewById(R.id.progressBar)
        elapsedTimeText = findViewById(R.id.elapsedTimeText)

        // Set up ProgressBar
        progressBar.max = 1000 // Set maximum value to represent 10 seconds (1000 * 10ms = 10 seconds)

        // Set up touch listener for detecting press events
        progressBar.setOnTouchListener { _, event ->
            when (event.action) {
                MotionEvent.ACTION_DOWN -> {
                    // User starts pressing, begin tracking time
                    if (!isPressed) {
                        startTime = System.currentTimeMillis()
                        isPressed = true
                        startTimer()  // Start the coroutine
                    }
                    true
                }
                MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                    // User releases press, stop tracking time
                    if (isPressed) {
                        isPressed = false
                        job?.cancel()  // Cancel the coroutine when press is released
                    }
                    true
                }
                else -> false
            }
        }
    }

    private fun startTimer() {
        // Launch a coroutine on the main thread
        job = CoroutineScope(Dispatchers.Main).launch {
            while (isPressed) {
                // Calculate elapsed time
                elapsedTime = System.currentTimeMillis() - startTime

                // Update the progress bar every 0.01 second
                val progress = (elapsedTime / 10).toInt() // Convert milliseconds to 0.01 second intervals
                progressBar.progress = progress

                // Update the elapsed time text (formatted to seconds with 2 decimal places)
                elapsedTimeText.text = "Elapsed Time: %.2f seconds".format(elapsedTime / 1000.0)

                // Delay for 10 milliseconds before updating again
                delay(10) // Suspends the coroutine for 10ms without blocking the main thread
            }
        }
    }

    override fun onStop() {
        super.onStop()
        job?.cancel()  // Cancel the coroutine when the activity is stopped
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="100dp"
        android:layout_marginStart="32dp"
        android:layout_marginEnd="32dp"
        android:max="1000"
        android:progress="0"
        android:indeterminate="false"
        style="@android:style/Widget.ProgressBar.Horizontal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/elapsedTimeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Elapsed Time: 0 seconds"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@id/progressBar"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```


<br>



### Example02: Coroutine
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

import android.annotation.SuppressLint
import android.os.Bundle
import android.view.MotionEvent
import android.widget.ProgressBar
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity() {
    private lateinit var progressBar: ProgressBar
    private lateinit var elapsedTimeText: TextView
    private var startTime: Long = 0
    private var isPressed = false
    private var elapsedTime: Long = 0
    private var job: Job? = null

    @SuppressLint("ClickableViewAccessibility")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Initialize views
        progressBar = findViewById(R.id.progressBar)
        elapsedTimeText = findViewById(R.id.elapsedTimeText)

        // Set up ProgressBar
        progressBar.max = 1000 // Set maximum value to represent 1 second (1000 ms = 1 second)

        // Set up touch listener for detecting press events
        progressBar.setOnTouchListener { _, event ->
            when (event.action) {
                MotionEvent.ACTION_DOWN -> {
                    // User starts pressing, begin tracking time
                    if (!isPressed) {
                        startTime = System.currentTimeMillis()
                        isPressed = true
                        startUpdatingProgress()
                    }
                    true
                }
                MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                    // User releases press, stop tracking time
                    if (isPressed) {
                        isPressed = false
                        stopUpdatingProgress()
                    }
                    true
                }
                else -> false
            }
        }
    }

    private fun startUpdatingProgress() {
        job = CoroutineScope(Dispatchers.Main).launch {
            while (isPressed) {
                // Calculate elapsed time
                elapsedTime = System.currentTimeMillis() - startTime

                // Update the progress bar
                val progress = (elapsedTime / 10) // Update every 10ms
                progressBar.progress = progress.toInt()

                // Update the elapsed time text
                elapsedTimeText.text = "Elapsed Time: ${elapsedTime / 1000.0} seconds"

                // Delay for 10ms before next update
                delay(10)
            }
        }
    }

    private fun stopUpdatingProgress() {
        job?.cancel() // Stop the coroutine when the user releases the press
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="100dp"
        android:layout_marginStart="32dp"
        android:layout_marginEnd="32dp"
        android:max="1000"
        android:progress="0"
        android:indeterminate="false"
        style="@android:style/Widget.ProgressBar.Horizontal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/elapsedTimeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Elapsed Time: 0 seconds"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@id/progressBar"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

