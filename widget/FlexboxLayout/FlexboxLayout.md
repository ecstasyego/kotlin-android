
## Examples
### Example01: *.kt
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
import android.widget.TextView
import androidx.activity.ComponentActivity
import com.google.android.flexbox.FlexboxLayout
import com.google.android.flexbox.FlexDirection
import com.google.android.flexbox.JustifyContent
import com.google.android.flexbox.AlignItems
import com.google.android.flexbox.FlexWrap

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val flexboxLayout = FlexboxLayout(this).apply {
            layoutParams = FlexboxLayout.LayoutParams(FlexboxLayout.LayoutParams.MATCH_PARENT, FlexboxLayout.LayoutParams.WRAP_CONTENT)
            flexDirection = FlexDirection.ROW // 수평 방향
            justifyContent = JustifyContent.SPACE_AROUND // 자식 뷰의 가로 배치
            alignItems = AlignItems.CENTER // 자식 뷰의 세로 정렬
            flexWrap = FlexWrap.WRAP // 화면을 넘으면 줄 바꿈
        }

        val textView1 = TextView(this).apply {
            text = "Item 1"
            setPadding(20, 20, 20, 20) // 텍스트에 패딩 추가
        }
        val textView2 = TextView(this).apply {
            text = "Item 2"
            setPadding(20, 20, 20, 20) // 텍스트에 패딩 추가
        }
        val textView3 = TextView(this).apply {
            text = "Item 3"
            setPadding(20, 20, 20, 20) // 텍스트에 패딩 추가
        }

        flexboxLayout.addView(textView1)
        flexboxLayout.addView(textView2)
        flexboxLayout.addView(textView3)

        setContentView(flexboxLayout)
    }
}
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.google.android.flexbox:flexbox:3.0.0")
}
```

<br>

### Example02: *.xml(findViewById)
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

    <com.google.android.flexbox.FlexboxLayout
        android:id="@+id/flexboxLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:flexDirection="row"
        app:justifyContent="space_around"
        app:alignItems="center"
        app:flexWrap="wrap"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        android:padding="10dp">

        <TextView
            android:id="@+id/textView1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Item 1"
            android:padding="20dp" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Item 2"
            android:padding="20dp" />

        <TextView
            android:id="@+id/textView3"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Item 3"
            android:padding="20dp" />

    </com.google.android.flexbox.FlexboxLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

