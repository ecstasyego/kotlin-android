## Examples
### Example01: *.kt
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

import android.os.Bundle
import android.view.ViewGroup
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = MyAdapter(List<String>(100){"ITEM ${it.toString()}"})
        setContentView(recyclerView)
    }

}

class MyAdapter(private val items: List<String>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
    companion object {
        const val VIEW00 = 0
        const val VIEW01 = 1
        const val VIEW02 = 2
    }

    inner class ViewHolder00(val textView: TextView) : RecyclerView.ViewHolder(textView)
    inner class ViewHolder01(val textView: TextView) : RecyclerView.ViewHolder(textView)
    inner class ViewHolder02(val textView: TextView) : RecyclerView.ViewHolder(textView)

    override fun getItemViewType(position: Int): Int {
        return when (position % 3) {
            0 -> VIEW00
            1 -> VIEW01
            else -> VIEW02
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
        return when (viewType) {
            VIEW00 -> {
                val textView = TextView(parent.context)
                textView.layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                textView.setPadding(16, 16, 16, 16)
                ViewHolder00(textView)
            }
            VIEW01 -> {
                val textView = TextView(parent.context)
                textView.layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                textView.setPadding(16, 16, 16, 16)
                textView.setTextAppearance(android.R.style.TextAppearance_Material_Large)
                ViewHolder01(textView)
            }
            VIEW02 -> {
                val textView = TextView(parent.context)
                textView.layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                textView.setPadding(16, 16, 16, 16)
                textView.setTextColor(parent.context.resources.getColor(android.R.color.holo_red_dark))
                textView.setBackgroundColor(parent.context.resources.getColor(android.R.color.holo_blue_light))
                ViewHolder02(textView)
            }
            else -> throw IllegalArgumentException("Invalid view type")
        }
    }


    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        when (holder) {
            is ViewHolder00 -> holder.textView.text = items[position]
            is ViewHolder01 -> holder.textView.text = items[position]
            is ViewHolder02 -> holder.textView.text = items[position]
        }
    }

    override fun getItemCount(): Int {
        return items.size
    }
}
```
