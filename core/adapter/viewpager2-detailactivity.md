

## Examples
### Example01: Dynamic View
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

import android.content.Intent
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
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import java.io.Serializable


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
        return MainFragment()
    }
}

class MainFragment : Fragment() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        recyclerView = RecyclerView(requireContext())
        recyclerView.layoutManager = LinearLayoutManager(requireContext())
        recyclerView.adapter = CustomAdapter(List(20) { Item("ITEM: $it") })
        return recyclerView
    }

}

class DetailActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val item = intent.getSerializableExtra("option") as? Item
        val view = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            addView(TextView(this@DetailActivity).apply {
                text = item?.option
            })
        }
        setContentView(view)
    }
}

class CustomAdapter(private val items: List<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    inner class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView

        init {
            itemView.setOnClickListener {
                val item = items[adapterPosition]  // Get the actual Item object based on the adapter position
                val intent = Intent(itemView.context, DetailActivity::class.java).apply {
                    putExtra("option", item)  // Pass the item to the DetailActivity
                }
                itemView.context.startActivity(intent)  // Start the DetailActivity
            }
        }

    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val textView = TextView(parent.context).apply{
            layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
            setPadding(16, 16, 16, 16)
        }
        return ViewHolder(textView)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String):Serializable
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
        <activity android:name=".DetailActivity"/>

    </application>
</manifest>
```

