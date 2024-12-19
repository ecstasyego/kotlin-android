
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
import android.widget.TableLayout
import android.widget.TableRow
import androidx.activity.ComponentActivity
import androidx.cardview.widget.CardView
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import android.view.Gravity
import android.view.ViewGroup
import android.widget.HorizontalScrollView
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

        // Create TableLayout
        val horizontalScrollView = HorizontalScrollView(parent.context)
        val tableLayout = TableLayout(parent.context)
        tableLayout.layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)

        // Create TableRows and TextViews
        for (row in 0 until 3) {
            val tableRow = TableRow(parent.context)
            for (col in 0 until 5) {
                val textView = TextView(parent.context).apply {
                    text = "[$row, $col]"
                    gravity = Gravity.START
                    setPadding(16, 16, 16, 16)
                }
                tableRow.addView(textView)
            }
            tableLayout.addView(tableRow)
        }

        // Add TableLayout to CardView
        horizontalScrollView.addView(tableLayout)
        cardView.addView(horizontalScrollView)
        return MyViewHolder(cardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        val horizontalScrollView = holder.cardView.getChildAt(0) as HorizontalScrollView
        val tableLayout = horizontalScrollView.getChildAt(0) as TableLayout
        for (row in 0 until tableLayout.childCount) {
            val tableRow = tableLayout.getChildAt(row) as TableRow
            for (col in 0 until tableRow.childCount) {
                val textView = tableRow.getChildAt(col) as TextView
                textView.text = "[$row, $col] ${items[position]}"
            }
        }
    }

    override fun getItemCount(): Int {
        return items.size
    }
}
```

