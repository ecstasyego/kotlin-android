

## Examples
### Example01: View + ViewModel()
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
import android.widget.Button
import android.widget.TextView
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel

class MainActivity : AppCompatActivity() {
    private val viewModel: MainViewModel by viewModels() // Lifecycle Awareness

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })

        val button: Button = findViewById(R.id.button)
        button.setOnClickListener {
            viewModel.updateText("Updated TextView")
        }
    }
}


class MainViewModel : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        _text.value = newText
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```

<br>


### Example02: ViewModel(View)
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
import android.widget.Button
import android.widget.TextView
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider

class MainActivity : AppCompatActivity() {
    private val button:Button by lazy {findViewById(R.id.button)}
    private val viewModel: MainViewModel by viewModels { MainViewModelFactory(button) } // Lifecycle Awareness

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        viewModel.updateText("Updated TextView")
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })

    }
}


class MainViewModel(private val button: Button) : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        button.setOnClickListener {
            _text.value = newText
        }

    }
}

class MainViewModelFactory(private val button: Button) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return MainViewModel(button) as T
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```

<br>


### Example03-1: ViewModel(*Views)
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
import android.widget.Button
import android.widget.TextView
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider

class MainActivity : AppCompatActivity() {
    private val button00:Button by lazy {findViewById(R.id.button00)}
    private val button01:Button by lazy {findViewById(R.id.button01)}
    private val viewModel: MainViewModel by viewModels { MainViewModelFactory(button00, button01) } // Lifecycle Awareness

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        viewModel.updateText("Updated TextView")
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })

    }
}


class MainViewModel(private val button00: Button, private val button01: Button) : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        button00.setOnClickListener {
            _text.value = newText + " from buton00"
        }
        button01.setOnClickListener {
            _text.value = newText + " from buton01"
        }
    }
}

class MainViewModelFactory(private val button00: Button, private val button01: Button) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return MainViewModel(button00, button01) as T
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />

    <Button
        android:id="@+id/button01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```

<br>

### Example03-2: ViewModel(*Views) with ViewModelProvider.Factory
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

import android.app.Application
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider

object AppContainer {
    private lateinit var applicationContext: Application
    fun initialize(application: Application) { applicationContext = application }
    fun getMainViewModel(vararg buttons: Button): MainViewModelFactory { return MainViewModelFactory(buttons[0], buttons[1]) }
}

class UserApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        AppContainer.initialize(this)
    }

    override fun onTerminate() {
        super.onTerminate()
    }
}


class MainActivity : AppCompatActivity() {
    private val button00:Button by lazy {findViewById(R.id.button00)}
    private val button01:Button by lazy {findViewById(R.id.button01)}
    private val viewModel: MainViewModel by viewModels { AppContainer.getMainViewModel(button00, button01) } // Lifecycle Awareness

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        viewModel.updateText("Updated TextView")
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })
    }
}


class MainViewModel(private val button00: Button, private val button01: Button) : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        button00.setOnClickListener {
            _text.value = newText + " from buton00"
        }
        button01.setOnClickListener {
            _text.value = newText + " from buton01"
        }
    }
}


class MainViewModelFactory(private val button00: Button, private val button01: Button) : ViewModelProvider.Factory {
    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        return MainViewModel(button00, button01) as T
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />

    <Button
        android:id="@+id/button01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:name=".UserApplication"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
</manifest>
```


<br>


### Example04-1: ViewModel(*Views) with koin
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
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import org.koin.dsl.module
import org.koin.androidx.viewmodel.dsl.viewModel
import org.koin.androidx.viewmodel.ext.android.viewModel as viewModel_ext
import org.koin.core.parameter.parametersOf
import org.koin.core.context.startKoin

val appModule = module {
    viewModel { (button00: Button, button01: Button) -> MainViewModel(button00, button01) }
}

class MainActivity : AppCompatActivity() {
    private val button00:Button by lazy {findViewById(R.id.button00)}
    private val button01:Button by lazy {findViewById(R.id.button01)}
    private val viewModel: MainViewModel by viewModel_ext { parametersOf(button00, button01) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
        startKoin {
            modules(appModule)
        }

        val textView: TextView = findViewById(R.id.textView)
        viewModel.updateText("Updated TextView")
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })

    }
}


class MainViewModel(private val button00: Button, private val button01: Button) : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        button00.setOnClickListener {
            _text.value = newText + " from buton00"
        }
        button01.setOnClickListener {
            _text.value = newText + " from buton01"
        }

    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />

    <Button
        android:id="@+id/button01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("io.insert-koin:koin-android:3.4.0")
    implementation("io.insert-koin:koin-android-compat:3.4.0")
}
```


<br>


### Example04-2:  ViewModel(*Views) with koin
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

import android.app.Application
import android.os.Bundle
import android.widget.Button
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import org.koin.android.ext.koin.androidContext
import org.koin.dsl.module
import org.koin.androidx.viewmodel.dsl.viewModel
import org.koin.androidx.viewmodel.ext.android.viewModel as viewModel_ext
import org.koin.core.parameter.parametersOf
import org.koin.core.context.startKoin


class UserApplication : Application() {
    val appModule = module {
        viewModel { (button00: Button, button01: Button) -> MainViewModel(button00, button01) }
    }

    override fun onCreate() {
        super.onCreate()
        startKoin {
            androidContext(this@UserApplication)
            modules(appModule)
        }
    }

    override fun onTerminate() {
        super.onTerminate()
    }
}


class MainActivity : AppCompatActivity() {
    private val button00:Button by lazy {findViewById(R.id.button00)}
    private val button01:Button by lazy {findViewById(R.id.button01)}
    private val viewModel: MainViewModel by viewModel_ext { parametersOf(button00, button01) }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        viewModel.updateText("Updated TextView")
        viewModel.text.observe(this, Observer { newText ->
            textView.text = newText
        })
    }
}


class MainViewModel(private val button00: Button, private val button01: Button) : ViewModel() {
    private val _text = MutableLiveData<String>()
    val text: LiveData<String> get() = _text

    init {
        _text.value = "TextView"
    }

    fun updateText(newText: String) {
        button00.setOnClickListener {
            _text.value = newText + " from buton00"
        }
        button01.setOnClickListener {
            _text.value = newText + " from buton01"
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="TextView"
        android:textSize="18sp" />

    <Button
        android:id="@+id/button00"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />

    <Button
        android:id="@+id/button01"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update TextView" />
</LinearLayout>
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:name=".UserApplication"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">

        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
</manifest>
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("io.insert-koin:koin-android:3.4.0")
    implementation("io.insert-koin:koin-android-compat:3.4.0")
}
```



