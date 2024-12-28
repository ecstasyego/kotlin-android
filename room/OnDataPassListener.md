## Examples
### Example01: mutableListOf<Map<String, Any>>()
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

import android.content.Context
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.Button
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction

class MainActivity : AppCompatActivity(), MainFragment.OnDataPassListener {
    private lateinit var main_layout:LinearLayout
    private var table = mutableListOf<Map<String, Any>>()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        main_layout = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            addView( FrameLayout(this@MainActivity).apply{ id = View.generateViewId() } )
        }
        setContentView(main_layout)

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()
    }

    override fun onDataPass(data: String) {
        table.add(mapOf("info01" to data))
        main_layout.addView(TextView(main_layout.context).apply{text = table[0]["info01"].toString()})
        setContentView(main_layout)
    }

}

class MainFragment : Fragment() {
    interface OnDataPassListener {
        fun onDataPass(data: String)
    }

    private var dataPassListener: OnDataPassListener? = null

    override fun onAttach(context: Context) {
        super.onAttach(context)
        dataPassListener = context as? OnDataPassListener
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
            addView( Button(requireContext()).apply{ text = "Click, Me!"})
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        val button = (view as LinearLayout).getChildAt(1) as Button
        button.setOnClickListener {
            dataPassListener?.onDataPass("Data from MainFragment")
        }
    }

    override fun onDetach() {
        super.onDetach()
        dataPassListener = null
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

