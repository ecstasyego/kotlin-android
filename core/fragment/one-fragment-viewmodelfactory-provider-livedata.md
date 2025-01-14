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
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider

class MainActivity : AppCompatActivity() {
    private lateinit var viewModel: MyViewModel

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
        viewModel = ViewModelProvider(this, MyViewModelFactory("Initial Data")).get(MyViewModel::class.java)
        viewModel.update("Data 1")
        viewModel.update("Data 2")
        viewModel.update("Data 3")
    }
}

class MainFragment : Fragment() {
    private lateinit var viewModel: MyViewModel

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply{addView( TextView(requireContext()).apply {text = "This is main fragment."} )}
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        viewModel = ViewModelProvider(requireActivity(), MyViewModelFactory("Initial Data")).get(MyViewModel::class.java)
        viewModel.item.observe(viewLifecycleOwner, Observer { newData ->
            Toast.makeText(requireContext(), newData, Toast.LENGTH_SHORT).show()
        })
    }
}

class MyViewModelFactory(private val initialData: String) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(MyViewModel::class.java)) {
            return MyViewModel(initialData) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}

class MyViewModel(private val initialData: String) : ViewModel() {
    private val _data = MutableLiveData<String>()
    val item: LiveData<String> get() = _data

    init {
        _data.value = initialData
    }

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

