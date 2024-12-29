# Fragment
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/activity_layout.xml
│   │       ├── res/layout/fragment_layout.xml
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
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply {
            addView( FrameLayout(this@MainActivity).apply {id = View.generateViewId()} )
        }
        setContentView(main_layout)

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    override fun onAttach(context: Context) {
        super.onAttach(context) // activity
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState) // fragment resources
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        val staticLayout = inflater.inflate(R.layout.fragment_layout, container, false)
        val dynamicLayout = LinearLayout(requireContext()).apply{addView( TextView(requireContext()).apply {text = "This is main fragment."} )}
        return dynamicLayout
    }
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return 
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState) // interaction with activity
    }

    override fun onStart() {
        super.onStart()
    }

    override fun onResume() {
        super.onResume()
    }

    override fun onPause() {
        super.onPause()
    }

    override fun onStop() {
        super.onStop()
    }

    override fun onDestroyView() {
        super.onDestroyView() // fragment view resource
    }

    override fun onDestroy() {
        super.onDestroy() // another fragment resources
    }

    override fun onDetach() {
        super.onDetach() // activity
    }
}
```


`activity_layout.xml`
```xml

```


`fragment_layout.xml`
```xml

```


`themes.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.MyApplication" parent="Theme.AppCompat.Light.DarkActionBar" />
</resources>
```


```kotlin

val context: Context = requireContext()
val mainActivity = activity as? MainActivity

```
