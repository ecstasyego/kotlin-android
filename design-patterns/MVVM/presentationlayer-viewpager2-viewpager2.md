
## Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/values/themes.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.os.Parcelable
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentActivity
import androidx.viewpager2.adapter.FragmentStateAdapter
import kotlinx.parcelize.Parcelize
import android.content.Context
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator

interface MainViewPagerNavigator {
    fun navigateTo(position: Int)
}

class MainActivity : AppCompatActivity(), MainViewPagerNavigator {
    lateinit var viewPager2: ViewPager2

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        viewPager2 = ViewPager2(this)
        viewPager2.adapter = MainViewPagerAdapter(this, CommonMainFragmentElement(null))
        setContentView(viewPager2)
    }

    override fun navigateTo(position: Int) {
        viewPager2.setCurrentItem(position, true)
    }
}


class MainViewPagerAdapter(fragmentActivity: FragmentActivity, val commonFragmentElement: CommonMainFragmentElement) : FragmentStateAdapter(fragmentActivity) {
    override fun getItemCount(): Int = 3

    override fun createFragment(position: Int): Fragment {
        return when (position) {
            0 -> MainFragment.newInstance(commonFragmentElement)
            1 -> MainFragment.newInstance(commonFragmentElement)
            else -> MainFragment.newInstance(commonFragmentElement)
        }
    }
}
```

```kotlin
interface SubViewPagerNavigator {
    fun navigateTo(position: Int)
}

@Parcelize
data class CommonMainFragmentElement(val param00:Int?): Parcelable

class MainFragment : Fragment(), SubViewPagerNavigator {
    private var navigator: MainViewPagerNavigator? = null
    private lateinit var cfe: CommonMainFragmentElement

    private lateinit var tabLayout: TabLayout
    private lateinit var subViewPager2: ViewPager2

    companion object {
        fun newInstance(commonFragmentElement: CommonMainFragmentElement): MainFragment {
            val fragment = MainFragment()
            val args = Bundle()
            args.putParcelable("CommonFragmentElement", commonFragmentElement)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is MainViewPagerNavigator) {
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
        tabLayout = TabLayout(requireContext())
        subViewPager2 = ViewPager2(requireContext())
        subViewPager2.adapter = SubViewPagerAdapter(this, CommonSubFragmentElement(null))

        return LinearLayout(requireContext()).apply {
            orientation = LinearLayout.VERTICAL
            addView(tabLayout)
            addView(subViewPager2)
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        TabLayoutMediator(tabLayout, subViewPager2) { tab, position ->
            tab.text = when(position) {
                0 -> "A"
                1 -> "B"
                else -> "C"
            }
        }.attach()

        navigator?.navigateTo(0)
        navigateTo(1)
    }

    override fun onDetach() {
        super.onDetach()
        navigator = null
    }

    override fun navigateTo(position: Int) {
        subViewPager2.setCurrentItem(position)
    }
}

class SubViewPagerAdapter(fragment: Fragment, val commonFragmentElement: CommonSubFragmentElement) : FragmentStateAdapter(fragment) {

    override fun getItemCount(): Int = 3

    override fun createFragment(position: Int): Fragment {
        return when(position) {
            0 -> SubFragment.newInstance(commonFragmentElement)
            1 -> SubFragment.newInstance(commonFragmentElement)
            else -> SubFragment.newInstance(commonFragmentElement)
        }
    }
}
```

```kotlin
@Parcelize
data class CommonSubFragmentElement(val param00:Int?): Parcelable

class SubFragment : Fragment() {
    private var mainNavigator: MainViewPagerNavigator? = null
    private var subNavigator: SubViewPagerNavigator? = null
    private lateinit var cfe: CommonSubFragmentElement

    companion object {
        fun newInstance(commonFragmentElement: CommonSubFragmentElement): SubFragment {
            val fragment = SubFragment()
            val args = Bundle()
            args.putParcelable("CommonFragmentElement", commonFragmentElement)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onAttach(context: Context) {
        super.onAttach(context)
        if (context is MainViewPagerNavigator) {
            mainNavigator = context
        } else {
            throw RuntimeException("$context must implement MainViewPagerNavigator")
        }

        val parent = parentFragment
        if (parent is SubViewPagerNavigator) {
            subNavigator = parent
        } else {
            throw RuntimeException("$context must implement PortfolioViewPagerNavigator")
        }

    }


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        cfe = arguments?.getParcelable("CommonFragmentElement")!!
    }


    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is sub fragment."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        mainNavigator?.navigateTo(0)
        subNavigator?.navigateTo(1)
    }

    override fun onDetach() {
        super.onDetach()
        mainNavigator = null
        subNavigator = null
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
