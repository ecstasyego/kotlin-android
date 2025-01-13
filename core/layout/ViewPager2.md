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


class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val frameLayout = FrameLayout(this).apply { layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT) }
        setContentView(frameLayout)

        val viewPager2 = ViewPager2(this).apply {
            layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT)
            adapter = FragmentAdapter(this@MainActivity)
            setPageTransformer { page, position -> page.alpha = 0.5f + (1 - Math.abs(position)) * 0.5f }
        }
        frameLayout.addView(viewPager2)
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

class Fragment00 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment00."} )
        }
    }
}

class Fragment01 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment01."} )
        }
    }
}

class Fragment02 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment02."} )
        }
    }
}
```

<br>

### Example02: *.kt
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

<br>

### Example03: *.xml(ViewPager2) + *.kt(Fragment)
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
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import androidx.viewpager2.adapter.FragmentStateAdapter
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView


class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val viewPager2 = findViewById<ViewPager2>(R.id.viewPager2)
        viewPager2.adapter = FragmentAdapter(this)
        viewPager2.setPageTransformer { page, position ->
            page.alpha = 0.5f + (1 - Math.abs(position)) * 0.5f
        }
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

class Fragment00 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment00."} )
        }
    }
}

class Fragment01 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment01."} )
        }
    }
}

class Fragment02 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment02."} )
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/viewPager2"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</FrameLayout>
```

<br>

### Example04: *.xml
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/fragment00.xml
│   │       ├── res/layout/fragment01.xml
│   │       ├── res/layout/fragment02.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import androidx.viewpager2.adapter.FragmentStateAdapter
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup


class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val viewPager2 = findViewById<ViewPager2>(R.id.viewPager2)
        viewPager2.adapter = FragmentAdapter(this)
        viewPager2.setPageTransformer { page, position ->
            page.alpha = 0.5f + (1 - Math.abs(position)) * 0.5f
        }
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

class Fragment00 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return inflater.inflate(R.layout.fragment00, container, false)
    }
}

class Fragment01 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return inflater.inflate(R.layout.fragment01, container, false)
    }
}

class Fragment02 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return inflater.inflate(R.layout.fragment02, container, false)
    }
}
```

`fragment00.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is fragment00."
        android:textSize="18sp" />
</LinearLayout>
```

`fragment01.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is fragment01."
        android:textSize="18sp" />
</LinearLayout>
```


`fragment02.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is fragment02."
        android:textSize="18sp" />
</LinearLayout>
```


