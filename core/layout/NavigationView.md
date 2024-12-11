## Examples
### Example01
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/menu/navigation_menu.xml
│   │       ├── res/values/themes.xml
│   │       ├── res/drawable/baseline_home_24.xml
│   │       ├── res/drawable/baseline_search_24.xml
│   │       ├── res/drawable/baseline_auto_graph_24.xml
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
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.drawerlayout.widget.DrawerLayout
import androidx.fragment.app.Fragment
import com.google.android.material.navigation.NavigationView

class MainActivity : AppCompatActivity() {
    private lateinit var drawerLayout: DrawerLayout
    private lateinit var navigationView: NavigationView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val fragment01 = Fragment01()
        val fragment02 = Fragment02()
        val fragment03 = Fragment03()

        drawerLayout = findViewById(R.id.drawerLayout)
        navigationView = findViewById(R.id.navigationView)

        replaceFragment(fragment01)

        navigationView.setNavigationItemSelectedListener { menuItem ->
            when (menuItem.itemId) {
                R.id.frag01 -> replaceFragment(fragment01)
                R.id.frag02 -> replaceFragment(fragment02)
                R.id.frag03 -> replaceFragment(fragment03)
            }

            drawerLayout.closeDrawers()
            true
        }
    }

    private fun replaceFragment(fragment: Fragment) {
        supportFragmentManager.beginTransaction()
            .replace(R.id.fragmentContainer, fragment)
            .commit()
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

class Fragment03 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment03."} )
        }
    }
}
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
        android:theme="@style/AppTheme"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

`main_layout.xml`
```xml
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:id="@+id/fragmentContainer"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/navigation_menu" />
</androidx.drawerlayout.widget.DrawerLayout>
```

`navigation_menu.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:id="@+id/frag01"
        android:icon="@drawable/baseline_home_24"
        android:title="HOME"
        />

    <item android:id="@+id/frag02"
        android:icon="@drawable/baseline_search_24"
        android:title="SEARCH"
        />

    <item android:id="@+id/frag03"
        android:icon="@drawable/baseline_auto_graph_24"
        android:title="GRAPH"
        />
</menu>
```

`baseline_home_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M10,20v-6h4v6h5v-8h3L12,3 2,12h3v8z"/>
</vector>
```

`baseline_search_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M15.5,14h-0.79l-0.28,-0.27C15.41,12.59 16,11.11 16,9.5 16,5.91 13.09,3 9.5,3S3,5.91 3,9.5 5.91,16 9.5,16c1.61,0 3.09,-0.59 4.23,-1.57l0.27,0.28v0.79l5,4.99L20.49,19l-4.99,-5zM9.5,14C7.01,14 5,11.99 5,9.5S7.01,5 9.5,5 14,7.01 14,9.5 11.99,14 9.5,14z"/>
</vector>
```

`baseline_auto_graph_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M14.06,9.94L12,9l2.06,-0.94L15,6l0.94,2.06L18,9l-2.06,0.94L15,12L14.06,9.94zM4,14l0.94,-2.06L7,11l-2.06,-0.94L4,8l-0.94,2.06L1,11l2.06,0.94L4,14zM8.5,9l1.09,-2.41L12,5.5L9.59,4.41L8.5,2L7.41,4.41L5,5.5l2.41,1.09L8.5,9zM4.5,20.5l6,-6.01l4,4L23,8.93l-1.41,-1.41l-7.09,7.97l-4,-4L3,19L4.5,20.5z"/>
</vector>
```

`themes.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" parent="Theme.MaterialComponents.Light.NoActionBar" />
</resources>
```




<br>


### Example02: with Navigation Header
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/nav_header.xml
│   │       ├── res/menu/navigation_menu.xml
│   │       ├── res/values/strings.xml
│   │       ├── res/values/themes.xml
│   │       ├── res/drawable/baseline_home_24.xml
│   │       ├── res/drawable/baseline_search_24.xml
│   │       ├── res/drawable/baseline_auto_graph_24.xml
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
import android.view.MenuItem
import android.view.View
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.ActionBarDrawerToggle
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.GravityCompat
import androidx.drawerlayout.widget.DrawerLayout
import androidx.fragment.app.Fragment
import com.google.android.material.navigation.NavigationView

class MainActivity : AppCompatActivity() {
    private lateinit var drawerLayout: DrawerLayout
    private lateinit var navigationView: NavigationView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val fragment01 = Fragment01()
        val fragment02 = Fragment02()
        val fragment03 = Fragment03()

        drawerLayout = findViewById(R.id.drawerLayout)
        navigationView = findViewById(R.id.navigationView)

        val toggle = ActionBarDrawerToggle(this, drawerLayout, R.string.open_drawer, R.string.close_drawer)
        drawerLayout.addDrawerListener(toggle)
        toggle.syncState()

        supportActionBar?.setDisplayHomeAsUpEnabled(true)
        replaceFragment(fragment01)

        navigationView.setNavigationItemSelectedListener { menuItem ->
            when (menuItem.itemId) {
                R.id.frag01 -> replaceFragment(fragment01)
                R.id.frag02 -> replaceFragment(fragment02)
                R.id.frag03 -> replaceFragment(fragment03)
            }
            drawerLayout.closeDrawers()
            true
        }
    }

    private fun replaceFragment(fragment: Fragment) {
        supportFragmentManager.beginTransaction()
            .replace(R.id.fragmentContainer, fragment)
            .commit()
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        return if (item.itemId == android.R.id.home) {
            drawerLayout.openDrawer(GravityCompat.START)
            true
        } else {
            super.onOptionsItemSelected(item)
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

class Fragment03 : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is fragment03."} )
        }
    }
}
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
        android:theme="@style/AppTheme"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/AppTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

`main_layout.xml`
```xml
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:id="@+id/fragmentContainer"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navigationView"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        app:headerLayout="@layout/nav_header"
        app:menu="@menu/navigation_menu" />
</androidx.drawerlayout.widget.DrawerLayout>
```

`navigation_menu.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:id="@+id/frag01"
        android:icon="@drawable/baseline_home_24"
        android:title="HOME"
        />

    <item android:id="@+id/frag02"
        android:icon="@drawable/baseline_search_24"
        android:title="SEARCH"
        />

    <item android:id="@+id/frag03"
        android:icon="@drawable/baseline_auto_graph_24"
        android:title="GRAPH"
        />
</menu>
```

`nav_header.xml`
```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp">

    <TextView
        android:id="@+id/navHeaderTitle"
        android:text="Navigation Drawer"
        android:textSize="20sp"
        android:textStyle="bold"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"/>
</LinearLayout>
```

`baseline_home_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M10,20v-6h4v6h5v-8h3L12,3 2,12h3v8z"/>
</vector>
```

`baseline_search_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M15.5,14h-0.79l-0.28,-0.27C15.41,12.59 16,11.11 16,9.5 16,5.91 13.09,3 9.5,3S3,5.91 3,9.5 5.91,16 9.5,16c1.61,0 3.09,-0.59 4.23,-1.57l0.27,0.28v0.79l5,4.99L20.49,19l-4.99,-5zM9.5,14C7.01,14 5,11.99 5,9.5S7.01,5 9.5,5 14,7.01 14,9.5 11.99,14 9.5,14z"/>
</vector>
```

`baseline_auto_graph_24.xml`
```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android" android:height="24dp" android:tint="#000000" android:viewportHeight="24" android:viewportWidth="24" android:width="24dp">
    <path android:fillColor="@android:color/white" android:pathData="M14.06,9.94L12,9l2.06,-0.94L15,6l0.94,2.06L18,9l-2.06,0.94L15,12L14.06,9.94zM4,14l0.94,-2.06L7,11l-2.06,-0.94L4,8l-0.94,2.06L1,11l2.06,0.94L4,14zM8.5,9l1.09,-2.41L12,5.5L9.59,4.41L8.5,2L7.41,4.41L5,5.5l2.41,1.09L8.5,9zM4.5,20.5l6,-6.01l4,4L23,8.93l-1.41,-1.41l-7.09,7.97l-4,-4L3,19L4.5,20.5z"/>
</vector>
```

`strings.xml`
```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="open_drawer">Open Drawer</string>
    <string name="close_drawer">Close Drawer</string>
</resources>
```

`themes.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="AppTheme" parent="Theme.MaterialComponents.Light.NoActionBar" />
</resources>
```


<br>

