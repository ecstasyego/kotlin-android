
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
import androidx.activity.viewModels
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    private val viewModel: MyViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = MyAdapter(List(100){"ITEM ${it}"}){ position ->
            viewModel.update(position)
        }

        viewModel.items.observe(this) { value ->
            (recyclerView.adapter as MyAdapter).submitChangedItems(value)
        }
        setContentView(recyclerView)
    }
}

class MyViewModel : ViewModel() {
    private val _items = MutableLiveData<List<String>>()
    val items: LiveData<List<String>> get() = _items

    init {
        _items.value = List(100) { "ITEM ${it}" }
    }

    fun update(position: Int) {
        _items.value = List(100){ "RANDOM: " + Random.nextInt(0, 100).toString() }
    }
}


class MyAdapter(private var items: List<String>, private val itemClickListener: (Int) -> Unit) : RecyclerView.Adapter<MyAdapter.MyViewHolder>() {
    class MyViewHolder(val textView: TextView, itemClickListener: (Int) -> Unit) : RecyclerView.ViewHolder(textView) {
        init {
            itemView.setOnClickListener {
                itemClickListener(bindingAdapterPosition)
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val textView = TextView(parent.context)
        textView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            ViewGroup.LayoutParams.WRAP_CONTENT
        )
        textView.setPadding(16, 16, 16, 16)
        return MyViewHolder(textView, itemClickListener)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.textView.text = items[position]
    }

    override fun getItemCount(): Int {
        return items.size
    }

    fun submitChangedItems(newItems: List<String>) {
        items = newItems // update items
        notifyDataSetChanged() // rerendering ViewHolder: onCreateViewHolder > onBindViewHolder
    }
}
```

