## Examples
### Example01: data class
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

        val data: List<Map<String, Any?>> = listOf(
            mapOf("key1" to "value1", "key2" to 123),
            mapOf("key1" to true, "key2" to null)
        )
        val fragment = MainFragment.newInstance(data)
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    lateinit var param: List<Map<String, Any?>>

    companion object {
        fun newInstance(param: List<Map<String, Any?>>): MainFragment {
            val fragment = MainFragment()
            val args = Bundle()
            args.putSerializable("ARGS_DATA", ArrayList(param))
            fragment.arguments = args
            return fragment
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        param = (arguments?.getSerializable("ARGS_DATA") as? List<Map<String, Any?>>)!!
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        val datum = param[0]["key1"]
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment with bundle(${datum})."} )
        }
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


