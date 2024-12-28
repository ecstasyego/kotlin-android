
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
import android.view.Gravity
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.drawerlayout.widget.DrawerLayout

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val drawerLayout = DrawerLayout(this)
        val drawerMenuLayout = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            layoutParams = DrawerLayout.LayoutParams( DrawerLayout.LayoutParams.MATCH_PARENT, DrawerLayout.LayoutParams.MATCH_PARENT).apply { gravity = Gravity.RIGHT }
            setBackgroundColor(resources.getColor(android.R.color.darker_gray))
        }
        drawerMenuLayout.addView(
            TextView(this).apply {
                text = "Menu Item 1"
                setPadding(16, 16, 16, 16)
                setOnClickListener {
                    Toast.makeText(this@MainActivity, "Menu Item 1 clicked", Toast.LENGTH_SHORT).show()
                    drawerLayout.closeDrawer(Gravity.RIGHT) // 드로어 닫기
                }
            }
        )
        drawerMenuLayout.addView(
            TextView(this).apply {
                text = "Menu Item 2"
                setPadding(16, 16, 16, 16)
                setOnClickListener {
                    Toast.makeText(this@MainActivity, "Menu Item 2 clicked", Toast.LENGTH_SHORT).show()
                    drawerLayout.closeDrawer(Gravity.RIGHT)
                }
            }
        )

        drawerLayout.addView(drawerMenuLayout)
        setContentView(drawerLayout)
    }
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
import android.view.Gravity
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.drawerlayout.widget.DrawerLayout
import android.widget.TextView

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.main_layout)

        val drawerLayout: DrawerLayout = findViewById(R.id.drawer_layout)
        val menuItem1: TextView = findViewById(R.id.textView00)
        val menuItem2: TextView = findViewById(R.id.textView01)

        menuItem1.setOnClickListener {
            Toast.makeText(this, "Menu Item 1 clicked", Toast.LENGTH_SHORT).show()
            drawerLayout.closeDrawer(Gravity.RIGHT)
        }

        menuItem2.setOnClickListener {
            Toast.makeText(this, "Menu Item 2 clicked", Toast.LENGTH_SHORT).show()
            drawerLayout.closeDrawer(Gravity.RIGHT)
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:layout_gravity="RIGHT"
        android:background="@android:color/darker_gray">

        <TextView
            android:id="@+id/textView00"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            android:text="Menu Item 1" />

        <TextView
            android:id="@+id/textView01"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp"
            android:text="Menu Item 2" />
    </LinearLayout>

</androidx.drawerlayout.widget.DrawerLayout>
```
