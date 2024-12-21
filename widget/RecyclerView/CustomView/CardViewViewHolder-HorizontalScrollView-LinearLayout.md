

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

import android.content.Context
import android.graphics.Color
import android.util.AttributeSet
import android.widget.HorizontalScrollView
import android.widget.LinearLayout
import android.widget.TextView
import android.view.Gravity
import androidx.cardview.widget.CardView
import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import android.view.ViewGroup

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = MyAdapter(List(100) { "ITEM ${it}" })
        setContentView(recyclerView)
    }
}

class MyAdapter(private val items: List<String>) : RecyclerView.Adapter<MyAdapter.MyViewHolder>() {
    class MyViewHolder(val customCardView: CustomCardView) : RecyclerView.ViewHolder(customCardView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val customCardView = CustomCardView(parent.context).apply {
            layoutParams = LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT, LinearLayout.LayoutParams.WRAP_CONTENT)
        }
        return MyViewHolder(customCardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.customCardView.populateColumns(10, items[position]) // 10 columns for each item
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

    private val horizontalScrollView: HorizontalScrollView
    private val linearLayout: LinearLayout

    init {
        setCardBackgroundColor(Color.WHITE)
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)

        horizontalScrollView = HorizontalScrollView(context)
        linearLayout = LinearLayout(context).apply {
            orientation = LinearLayout.HORIZONTAL
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        horizontalScrollView.addView(linearLayout)
        addView(horizontalScrollView)
    }

    fun populateColumns(columns: Int, item: String) {
        linearLayout.removeAllViews()  // Clear existing views before populating
        for (col in 0 until columns) {
            val textView = TextView(context).apply {
                text = "[$col] $item"
                gravity = Gravity.START
                setPadding(16, 16, 16, 16)
            }
            linearLayout.addView(textView)
        }
    }
}
```
