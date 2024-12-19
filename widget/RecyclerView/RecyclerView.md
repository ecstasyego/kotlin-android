## RecyclerView

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

class MyAdapter(private val items: List<String>) : RecyclerView.Adapter<MyAdapter.MyViewHolder>() {
    inner class MyViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val textView = TextView(parent.context)
        textView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            ViewGroup.LayoutParams.WRAP_CONTENT
        )
        textView.setPadding(16, 16, 16, 16)
        return MyViewHolder(textView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.textView.text = items[position]
    }

    override fun getItemCount(): Int {
        return items.size
    }
}
```


