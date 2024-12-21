## Android Application Architecture
![image](https://github.com/user-attachments/assets/b84ee85e-0e3e-4875-a8a3-68273a8a56dd)

- Principle
  - **Separation of Concerns**
  - **Modularization**
  - **UI-First Design**
    - (Design Pattern) MVVM: View > ViewModel > Model
      - Presentation Layer: View(Widget), ViewModel(LiveData)
      - Domain Layer: Repository(interface, abstract)
      - Data Layer: Model(Room)
- Android Architecture Components, AAC
  - **ViewModel**: The ViewModel is responsible for preparing and managing the data for the UI. It holds UI-related data in a lifecycle-conscious way and survives configuration changes (such as screen rotations).
  - **LiveData**: LiveData is an observable data holder class that allows UI components (like activities or fragments) to observe data changes in a lifecycle-aware way. It ensures that UI components are updated with the latest data when necessary.
  - **Room**: Room is an abstraction layer over SQLite that allows for more robust database access. It simplifies database interactions and ensures that database queries are performed in a background thread, making apps more responsive.
  - **Repository**: The Repository is a pattern that acts as a clean API for data access. It abstracts the data sources (network, cache, local database) and provides a unified interface for the ViewModel to fetch data. It helps in decoupling the data layer from the rest of the app.
  - **WorkManager**: WorkManager provides a simple and flexible way to manage background tasks that need to be guaranteed, such as syncing data with a server or running periodic tasks.
  - **Navigation Component**: The Navigation component helps in managing navigation and the back stack in a modular, consistent, and predictable way. It simplifies fragment transactions and deep linking in the app.
  - **Lifecycle**: The Android Lifecycle component ensures that the app's components (such as activities and fragments) are lifecycle-aware. It helps manage the UI-related tasks, so the app responds to lifecycle changes without worrying about the details of the lifecycle.
- Typical Android Application Architecture
  - **Presentation Layer** (UI Layer)
    - Activities/Fragments
    - ViewModel
  - **Domain Layer** (Business Logic Layer)
    - Use Cases/Interactors
    - Repository
  - **Data Layer**
    - Local Data Source (Room Database)
    - Remote Data Source (Network)
    - Cache
  - Dependency Injection (DI)
    - Dagger/Hilt
- Benefits of the Android Architecture
  - **Separation of Concerns**: The clear separation between the UI, business logic, and data layer ensures that each layer has a single responsibility.
  - **Testability**: The architecture promotes testability by decoupling components, making it easier to unit test the ViewModel, Repository, and other layers.
  - **Maintainability**: It makes it easier to maintain the app, as each component has a well-defined role. Developers can work on different layers independently.
  - **Lifecycle Awareness**: With components like ViewModel and LiveData, Android apps can handle lifecycle changes (such as screen rotations) smoothly without losing important data.




<br><br><br>
## Clean Architecture
### Base Structure: ViewModel
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


`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
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






<br><br><br>

## Design Patterns
- [Behavior]
  - Action > (View) > ...
    - ... > View

### MVC: Model + View + Controller
- [Behavior]
  - Action > (View) > Controller
    -  Model <-> Controller <-> View
- Controller : View
  - 1 : n
    - Controller : View01, View02, View03, ...
- (Dependency) **Model** : **View** 

<br>

### MVP: Model + View + Presenter
- [Behavior]
  - Action > View > Presenter > Model
    - Model > Presenter > View
- (Dependency) Presenter : View
  - 1 : 1
    - Presenter01 : View01
    - Presenter02 : View02
    - Presenter03 : View03
    - ...
- (Independency) **Model** : **View** 


<br>

### MVVM: Model + View + ViewModel
```
Model: Represents the data and business logic.
View: Represents the UI components (Activities and Fragments).
ViewModel: Acts as a bridge between the UI and the Model, exposing LiveData to the UI and handling business logic.
```

- [Behavior]
  - Action > View > ViewModel > Model
    - Model > ViewModel > View
- (Independency) ViewModel : View
  - 1 : n
    - ViewModel : View01, View02, View03, ...
- (Independency) **Model** : **View** 


<br>

### MVVM + DataBinding: Model + View + ViewModel + DataBinding

<br>

### MVI: Model + View + Intent

