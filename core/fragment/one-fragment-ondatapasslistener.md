## Examples


<br>


### Example02: Interface; Fragment > Activity
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
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction

class MainActivity : AppCompatActivity(), MainFragment.OnDataPassListener {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply{addView( FrameLayout(this@MainActivity).apply {id = View.generateViewId()} )}
        setContentView(main_layout)

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()
    }

    override fun onDataPass(data: String) {
        Toast.makeText(this, data, Toast.LENGTH_SHORT).show()
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
        dataPassListener?.onDataPass("Data from MainFragment")
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
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

