
## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/menu/bottom_navigation_menu.xml
│   │       ├── res/values/theme.xml
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
import android.util.AttributeSet
import android.view.Gravity
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import android.widget.FrameLayout
import androidx.viewpager2.adapter.FragmentStateAdapter
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.widget.Toolbar
import com.google.android.material.bottomnavigation.BottomNavigationView
import kotlin.math.abs

class MainActivity : AppCompatActivity() {
    lateinit var viewPagerFrame: ViewPagerFrame
    lateinit var toolbar:Toolbar
    lateinit var bottomNavigationView: BottomNavigationView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        viewPagerFrame = ViewPagerFrame(this)
        toolbar = Toolbar(this).apply { layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.WRAP_CONTENT).apply{gravity = Gravity.TOP}  }
        bottomNavigationView = BottomNavigationView(this).apply {
            layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.WRAP_CONTENT).apply { gravity = Gravity.BOTTOM }
            menuInflater.inflate(R.menu.bottom_navigation_menu, menu)

            // To synchronize the ViewPager2 with the BottomNavigationView
            setOnItemSelectedListener { item ->
                when (item.itemId) {
                    R.id.frag00 -> viewPagerFrame.viewPager2.currentItem = 0
                    R.id.frag01 -> viewPagerFrame.viewPager2.currentItem = 1
                    R.id.frag02 -> viewPagerFrame.viewPager2.currentItem = 2
                }
                true
            }
        }


        viewPagerFrame.viewPager2.registerOnPageChangeCallback(object : ViewPager2.OnPageChangeCallback() {
            override fun onPageSelected(position: Int) {
                super.onPageSelected(position)
                when (position) {
                    0 -> bottomNavigationView.selectedItemId = R.id.frag00
                    1 -> bottomNavigationView.selectedItemId = R.id.frag01
                    2 -> bottomNavigationView.selectedItemId = R.id.frag02
                }
            }
        })

        setSupportActionBar(toolbar)
        supportActionBar?.title = "Toolbar Title"
        toolbar.post { viewPagerFrame.viewPager2.setPadding(0, toolbar.height, 0, 0) }

        viewPagerFrame.addView(toolbar)
        viewPagerFrame.addView(bottomNavigationView)
        setContentView(viewPagerFrame)
    }
}

class ViewPagerFrame @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0) : FrameLayout(context, attrs, defStyleAttr) {
    val viewPager2: ViewPager2 = ViewPager2(context).apply {
        layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT)
        adapter = FragmentAdapter(context as FragmentActivity)
        setPageTransformer { page, position ->
            page.alpha = 0.5f + (1 - abs(position)) * 0.5f
        }
    }

    class FragmentAdapter(fragmentActivity: FragmentActivity) : FragmentStateAdapter(fragmentActivity) {
        override fun getItemCount(): Int = 3

        override fun createFragment(position: Int): Fragment {
            return when (position) {
                0 -> Fragment00()
                1 -> Fragment01()
                else -> Fragment02()
            }
        }
    }

    init {
        layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT)
        addView(viewPager2)
    }
}


class Fragment00 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment00." })
        }
    }
}

class Fragment01 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment01." })
        }
    }
}

class Fragment02 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment02." })
        }
    }
}
```

`bottom_navigation_menu.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:id="@+id/frag00"
        android:icon="@drawable/baseline_home_24"
        android:title="HOME"
        />

    <item android:id="@+id/frag01"
        android:icon="@drawable/baseline_search_24"
        android:title="SEARCH"
        />

    <item android:id="@+id/frag02"
        android:icon="@drawable/baseline_auto_graph_24"
        android:title="GRAPH"
        />
</menu>
```

`theme.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.MyApplication" parent="Theme.MaterialComponents.Light.NoActionBar" />
</resources>
```



<br>



### Example02
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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import android.widget.FrameLayout
import androidx.viewpager2.adapter.FragmentStateAdapter
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView
import kotlin.math.abs

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val viewPagerFrame = ViewPagerFrame(this)
        setContentView(viewPagerFrame)
    }
}

class ViewPagerFrame @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyleAttr: Int = 0) : FrameLayout(context, attrs, defStyleAttr) {

    class FragmentAdapter(fragmentActivity: FragmentActivity) : FragmentStateAdapter(fragmentActivity) {
        override fun getItemCount(): Int = 3

        override fun createFragment(position: Int): Fragment {
            return when (position) {
                0 -> Fragment00()
                1 -> Fragment01()
                else -> Fragment02()
            }
        }
    }

    init {
        layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT)

        val viewPager2 = ViewPager2(context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT)
            adapter = FragmentAdapter(context as FragmentActivity)
            setPageTransformer { page, position ->
                page.alpha = 0.5f + (1 - abs(position)) * 0.5f
            }
        }
        addView(viewPager2)
    }
}


class Fragment00 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment00." })
        }
    }
}

class Fragment01 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment01." })
        }
    }
}

class Fragment02 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView(TextView(requireContext()).apply { text = "This is fragment02." })
        }
    }
}
```
