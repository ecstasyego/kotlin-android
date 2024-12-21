
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
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.RecyclerView
import androidx.recyclerview.widget.LinearLayoutManager
import android.view.ViewGroup
import android.content.Context
import android.graphics.Color
import android.util.AttributeSet
import androidx.cardview.widget.CardView
import android.view.Gravity
import android.widget.LinearLayout.LayoutParams
import android.widget.HorizontalScrollView
import android.widget.TableLayout
import android.widget.TableRow


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
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
            addTableRows(3, 5) // Add 3 rows and 5 columns
        }
        return MyViewHolder(customCardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.customCardView.updateTableData(items, 3, 5)
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
    private val tableLayout: TableLayout

    init {
        setCardBackgroundColor(Color.WHITE)
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)

        horizontalScrollView = HorizontalScrollView(context)
        tableLayout = TableLayout(context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        horizontalScrollView.addView(tableLayout)
        addView(horizontalScrollView)
    }

    fun addTableRows(rows: Int, columns: Int) {
        for (row in 0 until rows) {
            val tableRow = TableRow(context)
            for (col in 0 until columns) {
                val textView = TextView(context).apply {
                    text = "[$row, $col]"
                    gravity = Gravity.START
                    setPadding(16, 16, 16, 16)
                }
                tableRow.addView(textView)
            }
            tableLayout.addView(tableRow)
        }
    }

    fun updateTableData(items: List<String>, rowCount: Int, columnCount: Int) {
        for (row in 0 until tableLayout.childCount) {
            val tableRow = tableLayout.getChildAt(row) as TableRow
            for (col in 0 until tableRow.childCount) {
                val textView = tableRow.getChildAt(col) as TextView
                textView.text = "[$row, $col] ${items.getOrElse(row) { "" }}"
            }
        }
    }
}
```

