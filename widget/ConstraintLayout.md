## ConstraintLayout

https://developer.android.com/reference/androidx/constraintlayout/widget/ConstraintLayout

### XML attributes
- Layout Constraints    
    - app:layout_constraintLeft_toLeftOf
    - app:layout_constraintLeft_toRightOf
    - app:layout_constraintRight_toLeftOf
    - app:layout_constraintRight_toRightOf
    - app:layout_constraintTop_toTopOf
    - app:layout_constraintTop_toBottomOf
    - app:layout_constraintBottom_toTopOf
    - app:layout_constraintBottom_toBottomOf
    - app:layout_constraintBaseline_toBaselineOf
    - app:layout_constraintStart_toEndOf
    - app:layout_constraintStart_toStartOf
    - app:layout_constraintEnd_toStartOf
    - app:layout_constraintEnd_toEndOf
- Margins
    - android:layout_marginStart
    - android:layout_marginEnd
    - android:layout_marginLeft
    - android:layout_marginTop
    - android:layout_marginRight
    - android:layout_marginBottom
    - layout_marginBaseline
- Margins when connected to a GONE widget
    - app:layout_goneMarginStart
    - app:layout_goneMarginEnd
    - app:layout_goneMarginLeft
    - app:layout_goneMarginTop
    - app:layout_goneMarginRight
    - app:layout_goneMarginBottom
    - app:layout_goneMarginBaseline
- Bias
    - app:layout_constraintHorizontal_bias
    - app:layout_constraintVertical_bias
- Circular positioning
    - app:layout_constraintCircle : references another widget id
    - app:layout_constraintCircleRadius : the distance to the other widget center
    - app:layout_constraintCircleAngle : which angle the widget should be at (in degrees, from 0 to 360)
- Minimum dimensions
    - android:minWidth set the minimum width for the layout
    - android:minHeight set the minimum height for the layout
    - android:maxWidth set the maximum width for the layout
    - android:maxHeight set the maximum height for the layout


---


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

import android.os.Bundle
import android.view.View
import android.widget.Button
import androidx.activity.ComponentActivity
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.constraintlayout.widget.ConstraintSet

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val constraintLayout = ConstraintLayout(this).apply { id = View.generateViewId() }
        val button = Button(this).apply { id = View.generateViewId(); text = "Click Me" }
        constraintLayout.addView(button)

        val constraintSet = ConstraintSet()
        constraintSet.clone(constraintLayout)
        constraintSet.connect(button.id, ConstraintSet.TOP, constraintLayout.id, ConstraintSet.TOP, 100)
        constraintSet.connect(button.id, ConstraintSet.START, constraintLayout.id, ConstraintSet.START, 100)
        constraintSet.applyTo(constraintLayout)

        setContentView(constraintLayout)
    }
}
```


<br>

### Usage: *.xml(findViewById)
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
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline_top_horizontal"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintGuide_percent="0.1" />

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 1"
        app:layout_constraintTop_toBottomOf="@id/guideline_top_horizontal"
        app:layout_constraintBottom_toTopOf="@id/textView"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="50dp"
        android:layout_marginBottom="50dp"
        android:layout_marginStart="50dp"
        android:layout_marginEnd="50dp" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!"
        app:layout_constraintTop_toBottomOf="@id/button1"
        app:layout_constraintBottom_toTopOf="@id/button2"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button 2"
        app:layout_constraintTop_toBottomOf="@id/textView"
        app:layout_constraintBottom_toTopOf="@id/guideline_bottom_horizontal"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />

    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideline_bottom_horizontal"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintGuide_percent="0.8" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

![image](https://github.com/user-attachments/assets/a583f2ec-e5ba-45c4-89ef-5a93507fc338)




