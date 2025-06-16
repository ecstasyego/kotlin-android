
## Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── java/com/example/myapplication/Fragment00.kt
│   │       ├── java/com/example/myapplication/Fragment01.kt
│   │       ├── java/com/example/myapplication/Fragment02.kt
│   │       ├── res/values/themes.xml
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
import android.os.Parcelable
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
import kotlinx.parcelize.Parcelize

interface ViewPagerNavigator {
    fun navigateTo(position: Int)
}

class MainActivity : AppCompatActivity(), ViewPagerNavigator {
    lateinit var viewPager2: ViewPager2

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        viewPager2 = ViewPager2(this)
        viewPager2.adapter = FragmentAdapter(this, CommonFragmentElement(null))
        setContentView(viewPager2)
    }

    override fun navigateTo(position: Int) {
        viewPager2.setCurrentItem(position, true)
    }
}


class FragmentAdapter(fragmentActivity: FragmentActivity, val commonFragmentElement: CommonFragmentElement) : FragmentStateAdapter(fragmentActivity) {
    override fun getItemCount(): Int = 3

    override fun createFragment(position: Int): Fragment {
        return when (position) {
            0 -> Fragment00.newInstance(commonFragmentElement)
            1 -> Fragment01.newInstance(commonFragmentElement)
            else -> Fragment02.newInstance(commonFragmentElement)
        }
    }
}

@Parcelize
data class CommonFragmentElement(val param00:Int?): Parcelable
```

`Fragment00.kt`
```kotlin
class Fragment00 : Fragment() {
    private var navigator: ViewPagerNavigator? = null
    private lateinit var cfe: CommonFragmentElement

    companion object {
        fun newInstance(commonFragmentElement: CommonFragmentElement): Fragment00 {
            val fragment = Fragment00()
            val args = Bundle()
            args.putParcelable("CommonFragmentElement", commonFragmentElement)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is ViewPagerNavigator) {
            navigator = context
        } else {
            throw RuntimeException("$context must implement ViewPagerNavigator")
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        cfe = arguments?.getParcelable("CommonFragmentElement")!!
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment00."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

    override fun onDetach() {
        super.onDetach()
        navigator = null
    }
}
```

`Fragment01.kt`
```kotlin
class Fragment01 : Fragment() {
    private var navigator: ViewPagerNavigator? = null
    private lateinit var cfe: CommonFragmentElement

    companion object {
        fun newInstance(commonFragmentElement: CommonFragmentElement): Fragment01 {
            val fragment = Fragment01()
            val args = Bundle()
            args.putParcelable("CommonFragmentElement", commonFragmentElement)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is ViewPagerNavigator) {
            navigator = context
        } else {
            throw RuntimeException("$context must implement ViewPagerNavigator")
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        cfe = arguments?.getParcelable("CommonFragmentElement")!!
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment01."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

    override fun onDetach() {
        super.onDetach()
        navigator = null
    }

}

```

`Fragment02.kt`
```kotlin
class Fragment02 : Fragment() {
    private var navigator: ViewPagerNavigator? = null
    private lateinit var cfe: CommonFragmentElement

    companion object {
        fun newInstance(commonFragmentElement: CommonFragmentElement): Fragment02 {
            val fragment = Fragment02()
            val args = Bundle()
            args.putParcelable("CommonFragmentElement", commonFragmentElement)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is ViewPagerNavigator) {
            navigator = context
        } else {
            throw RuntimeException("$context must implement ViewPagerNavigator")
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        cfe = arguments?.getParcelable("CommonFragmentElement")!!
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment02."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

    override fun onDetach() {
        super.onDetach()
        navigator = null
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

`build.gradle.kts(APP-LEVEL)`
```kotlin
plugins {
    id("kotlin-kapt") // for Room annotation processing
    id("kotlin-parcelize") //
}

android {
    kapt {
        correctErrorTypes = true
    }
}

dependencies {
    implementation("androidx.activity:activity-ktx:1.7.2")  // Required for activityViewModels()
    implementation("androidx.fragment:fragment-ktx:1.6.1")  // Fragment KTX for enhanced features
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")  // ViewModel KTX
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.1")  // LiveData KTX

    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3")

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```
