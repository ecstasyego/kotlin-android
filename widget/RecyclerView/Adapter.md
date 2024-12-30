## RecyclerView

## Examples
### Example01: Adapter<List<String>>
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(List(20) { "Item #$it" })
        setContentView(recyclerView)
    }
}

class CustomAdapter(private val items: List<String>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position]
    }

    override fun getItemCount(): Int = items.size
}
```


<br>



### Example02: Adapter<List<Item>>
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(List(20) { Item("ITEM: $it") })
        setContentView(recyclerView)
    }
}

class CustomAdapter(private val items: List<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```

<br>

### Example03: notify
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(MutableList(20) { Item("ITEM: $it") })
        setContentView(recyclerView)

        (recyclerView.adapter as CustomAdapter).itemChanged()
        (recyclerView.adapter as CustomAdapter).dataSetChanged()
        (recyclerView.adapter as CustomAdapter).itemRemoved()
        (recyclerView.adapter as CustomAdapter).itemInserted()
        (recyclerView.adapter as CustomAdapter).itemRangeInserted()
    }
}

class CustomAdapter(private var items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size

    fun itemChanged(){
        val position = 1
        items[position] = Item("itemChanged")
        notifyItemChanged(position)
    }

    fun dataSetChanged(){
        items = MutableList(30){ Item("dataSetChanged $it")}
        notifyDataSetChanged()
    }

    fun itemInserted(){
        val position:Int = 1
        items.add(position, Item("Inserted ITEM"))
        notifyItemInserted(position)
    }

    fun itemRemoved(){
        val position:Int = 1
        items.removeAt(position)
        notifyItemRemoved(position)
    }

    fun itemRangeInserted(){
        val position:Int = 1
        val itemCount:Int = 5
        items.addAll(position, List(itemCount){ Item("Inserted ITEM $it") })
        notifyItemRangeInserted(position, itemCount)
    }
}

data class Item(var option:String)
```


<br>


### Example04: notifyItemChanged(position: Int)
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val items = MutableList(20) { Item("ITEM: $it") }
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(items)
        setContentView(recyclerView)

        val position:Int = 1
        items[position] = Item("Updated ITEM: $position")
        (recyclerView.adapter as CustomAdapter).notifyItemChanged(position)

    }
}

class CustomAdapter(private val items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```


<br>


### Example05: notifyDataSetChanged()
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val items = MutableList(20) { Item("ITEM: $it") }
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(items)
        setContentView(recyclerView)

        items.clear()
        items.addAll(List(10) { Item("ITEM: $it") })
        items.reverse()
        (recyclerView.adapter as CustomAdapter).notifyDataSetChanged()
    }
}

class CustomAdapter(private val items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```


<br>


### Example06: notifyItemInserted(position: Int)
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val items = MutableList(20) { Item("ITEM: $it") }
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(items)
        setContentView(recyclerView)

        val position:Int = 1
        items.add(position, Item("Inserted ITEM"))
        (recyclerView.adapter as CustomAdapter).notifyItemInserted(position)
    }
}

class CustomAdapter(private val items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```



<br>


### Example07: notifyItemRemoved(position: Int)
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val items = MutableList(20) { Item("ITEM: $it") }
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(items)
        setContentView(recyclerView)

        val position:Int = 1
        items.removeAt(position)
        (recyclerView.adapter as CustomAdapter).notifyItemRemoved(position)
    }
}

class CustomAdapter(private val items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```



<br>


### Example08: notifyItemRangeInserted(positionStart: Int, itemCount: Int)
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
import androidx.activity.ComponentActivity
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

class MainActivity : ComponentActivity() {
    lateinit var  recyclerView: RecyclerView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val items = MutableList(20) { Item("ITEM: $it") }
        recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = CustomAdapter(items)
        setContentView(recyclerView)

        val position:Int = 1
        val itemCount:Int = 5
        items.addAll(position, List(itemCount){ Item("Inserted ITEM $it") })
        (recyclerView.adapter as CustomAdapter).notifyItemRangeInserted(position, itemCount)
    }
}

class CustomAdapter(private val items: MutableList<Item>) : RecyclerView.Adapter<CustomAdapter.ViewHolder>() {
    class ViewHolder(itemView: android.view.View) : RecyclerView.ViewHolder(itemView) {
        val textView: TextView = itemView as TextView
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(
            TextView(parent.context).apply{
                layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
                setPadding(16, 16, 16, 16)
            }
        )
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items[position].option
    }

    override fun getItemCount(): Int = items.size
}

data class Item(var option:String)
```




<br>


### Example09: notifyItemRangeChanged(positionStart: Int, itemCount: Int)
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
```



<br>


### Example10: notifyItemMoved(fromPosition: Int, toPosition: Int)
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
```

