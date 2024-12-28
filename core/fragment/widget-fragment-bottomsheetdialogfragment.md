## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/value/themes.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.fragment.app.FragmentActivity
import com.google.android.material.bottomsheet.BottomSheetDialogFragment

class MainActivity : FragmentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        val bottomSheetFragment = MenuBottomSheetFragment()
        bottomSheetFragment.show(supportFragmentManager, bottomSheetFragment.tag)
    }
}

class MenuBottomSheetFragment : BottomSheetDialogFragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        val menuItem1 = TextView(context).apply {
            text = "Menu Item 1"
            setPadding(16, 16, 16, 16)
            setOnClickListener {
                Toast.makeText(activity, "Menu Item 1 clicked", Toast.LENGTH_SHORT).show()
                dismiss() // Dismiss the bottom sheet when clicked
            }
        }

        val menuItem2 = TextView(context).apply {
            text = "Menu Item 2"
            setPadding(16, 16, 16, 16)
            setOnClickListener {
                Toast.makeText(activity, "Menu Item 2 clicked", Toast.LENGTH_SHORT).show()
                dismiss() // Dismiss the bottom sheet when clicked
            }
        }

        val layout = LinearLayout(context).apply {
            orientation = LinearLayout.VERTICAL
            addView(menuItem1)
            addView(menuItem2)
        }

        return layout // Return the layout to show in the bottom sheet
    }
}
```


`themes.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.MyApplication" parent="Theme.AppCompat.Light.DarkActionBar" />
</resources>
```
