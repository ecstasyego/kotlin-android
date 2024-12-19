## Example
### Example01
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
import android.view.LayoutInflater
import androidx.activity.ComponentActivity
import com.example.myapplication.databinding.MainLayoutBinding // *.xml > com.example.myapplication.databinding.*Binding 

class MainActivity : ComponentActivity() {
    private lateinit var binding: MainLayoutBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = MainLayoutBinding.inflate(layoutInflater)
        binding = MainLayoutBinding.inflate(LayoutInflater.from(this))
        binding.root
        binding.textView
        binding.button

        setContentView(binding.root)
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
        android:text="Hello, World!"
        android:textSize="20sp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

</LinearLayout>
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
android {
    buildFeatures {
        viewBinding = true
    }
}
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
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/item_layout.xml
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
import androidx.activity.ComponentActivity

// *.xml > com.example.myapplication.databinding.*Binding
import com.example.myapplication.databinding.MainLayoutBinding // main_layout.xml
import com.example.myapplication.databinding.ItemLayoutBinding // item_layout.xml

class MainActivity : ComponentActivity() {
    private lateinit var mainLayoutBinding: MainLayoutBinding
    private lateinit var itemLayoutBinding: ItemLayoutBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainLayoutBinding = MainLayoutBinding.inflate(layoutInflater)
        mainLayoutBinding = MainLayoutBinding.inflate(LayoutInflater.from(this))
        mainLayoutBinding.root
        mainLayoutBinding.mainLayout

        itemLayoutBinding = ItemLayoutBinding.inflate(layoutInflater)
        itemLayoutBinding = ItemLayoutBinding.inflate(LayoutInflater.from(this))
        itemLayoutBinding.root
        itemLayoutBinding.cardView
        itemLayoutBinding.textView

        mainLayoutBinding.root.addView(itemLayoutBinding.root)
        setContentView(mainLayoutBinding.root)
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</LinearLayout>
```

`item_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/cardView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp"
    android:layout_marginBottom="8dp"
    android:radius="8dp">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:padding="16dp" />

</androidx.cardview.widget.CardView>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
android {
    buildFeatures {
        viewBinding = true
    }
}
```


