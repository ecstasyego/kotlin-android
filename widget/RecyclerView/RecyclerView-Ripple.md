
## RecyclerView

## Examples
### Example01: Basic
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/drawable/ripple_background.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  widget: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = RecyclerView(this)
        widget.layoutManager = LinearLayoutManager(this)
        widget.adapter = CustomAdapter(List(20) { Item("ITEM: $it") })
        setContentView(widget)
    }
}

class CustomAdapter(private val items: List<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val textView = TextView(parent.context)
        textView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            ViewGroup.LayoutParams.WRAP_CONTENT
        )
        textView.isClickable = true
        textView.isFocusable = true
        textView.setBackgroundResource(R.drawable.ripple_background)
        textView.setPadding(16, 16, 16, 16)
        return ViewHolder(textView)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```


