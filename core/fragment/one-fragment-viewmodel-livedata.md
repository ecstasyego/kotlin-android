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
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.activity.viewModels  // Make sure to import this
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.fragment.app.activityViewModels
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel

class MainActivity : AppCompatActivity() {
    private val viewModel: MyViewModel by viewModels()  // Access ViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val mainLayout = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            addView(FrameLayout(this@MainActivity).apply{ id = View.generateViewId() })
        }

        setContentView(mainLayout)

        // Add the fragment dynamically
        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(mainLayout.getChildAt(0).id, fragment)
        transaction.commit()

        // Update ViewModel with some data
        viewModel.update("Data 1")
        viewModel.update("Data 2")
        viewModel.update("Data 3")
    }
}

class MainFragment : Fragment() {
    private val viewModel: MyViewModel by activityViewModels()  // Access shared ViewModel

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply{addView( TextView(requireContext()).apply {text = "This is main fragment."} )}
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel.item.observe(viewLifecycleOwner, Observer { newData ->
            Toast.makeText(requireContext(), newData, Toast.LENGTH_SHORT).show()
        })
    }
}

class MyViewModel : ViewModel() {
    private val _data = MutableLiveData<String>()
    val item: LiveData<String> get() = _data

    fun update(newData: String) {
        _data.value = newData
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
dependencies {
    implementation("androidx.activity:activity-ktx:1.7.2")  // Required for activityViewModels()
    implementation("androidx.fragment:fragment-ktx:1.6.1")  // Fragment KTX for enhanced features
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")  // ViewModel KTX
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.1")  // LiveData KTX
}
```


<br>


### Example02: *.xml
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
import android.view.View
import android.widget.Toast
import androidx.activity.viewModels  // Make sure to import this
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.fragment.app.activityViewModels
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel

class MainActivity : AppCompatActivity(R.layout.activity_layout) {
    private val viewModel: MyViewModel by viewModels()  // Access ViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Add the fragment dynamically
        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(R.id.fragment_container, fragment)
        transaction.commit()

        // Update ViewModel with some data
        viewModel.update("Data 1")
        viewModel.update("Data 2")
        viewModel.update("Data 3")
    }
}

class MainFragment : Fragment(R.layout.fragment_layout) {
    private val viewModel: MyViewModel by activityViewModels()  // Access shared ViewModel

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel.item.observe(viewLifecycleOwner, Observer { newData ->
            Toast.makeText(requireContext(), newData, Toast.LENGTH_SHORT).show()
        })
    }
}

class MyViewModel : ViewModel() {
    private val _data = MutableLiveData<String>()
    val item: LiveData<String> get() = _data

    fun update(newData: String) {
        _data.value = newData
    }
}
```

`activity_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <FrameLayout
        android:id="@+id/fragment_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

`fragment_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="This is main fragment."
        android:textSize="18sp" />

</LinearLayout>
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
dependencies {
    implementation("androidx.activity:activity-ktx:1.7.2")  // Required for activityViewModels()
    implementation("androidx.fragment:fragment-ktx:1.6.1")  // Fragment KTX for enhanced features
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")  // ViewModel KTX
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.1")  // LiveData KTX
}
```


