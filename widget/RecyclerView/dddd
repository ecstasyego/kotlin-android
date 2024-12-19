

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

import android.graphics.Color
import android.os.Bundle
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.cardview.widget.CardView
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import android.view.Gravity
import android.view.ViewGroup
import android.widget.LinearLayout
import android.widget.LinearLayout.LayoutParams

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

    class MyViewHolder(val cardView: CardView) : RecyclerView.ViewHolder(cardView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        // Create CardView
        val cardView = CardView(parent.context)
        val cardLayoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        cardView.layoutParams = cardLayoutParams
        cardView.setCardBackgroundColor(Color.WHITE)
        cardView.radius = 12f
        cardView.setCardElevation(4f)
        cardView.setContentPadding(16, 16, 16, 16)

        // Create LinearLayout
        val linearLayout = LinearLayout(parent.context)
        linearLayout.layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        linearLayout.orientation = LinearLayout.HORIZONTAL

        // Create TextViews
        for (col in 0 until 10) {
            val textView = TextView(parent.context).apply {
                text = "[$col]"
                gravity = Gravity.START
                setPadding(16, 16, 16, 16)
                layoutParams = LayoutParams(0, LayoutParams.WRAP_CONTENT, 1f)
            }
            linearLayout.addView(textView)
        }

        // Add TableLayout to CardView
        cardView.addView(linearLayout)
        return MyViewHolder(cardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        val linearLayout = holder.cardView.getChildAt(0) as LinearLayout
        for (col in 0 until linearLayout.childCount) {
            val textView = linearLayout.getChildAt(col) as TextView
            textView.text = "[$col] ${items[position]}"
        }
    }

    override fun getItemCount(): Int {
        return items.size
    }
}
```
