
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
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import android.content.Context
import android.graphics.Color
import android.util.AttributeSet
import android.view.Gravity
import android.widget.TextView
import androidx.cardview.widget.CardView
import android.widget.LinearLayout

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

    class MyViewHolder(val cardView: CustomCardView) : RecyclerView.ViewHolder(cardView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        return MyViewHolder(CustomCardView(parent.context))
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.cardView.textView.text = items[position]
    }

    override fun getItemCount(): Int {
        return items.size
    }
}

class CustomCardView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : CardView(context, attrs, defStyleAttr) {

    val textView: TextView

    init {
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)
        setCardBackgroundColor(Color.WHITE)

        textView = TextView(context).apply {
            layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
            )
            textSize = 16f
            setPadding(16, 16, 16, 16)
            gravity = Gravity.START
        }

        addView(textView)
    }

    fun setText(text: String) {
        textView.text = text
    }
}
```
