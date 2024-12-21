

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
import android.widget.TextView
import android.view.Gravity
import androidx.cardview.widget.CardView
import androidx.constraintlayout.widget.ConstraintLayout
import android.os.Bundle
import android.view.View
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
            layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
        }
        return MyViewHolder(customCardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.customCardView.setupTextViews(items[position])
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

    private val constraintLayout: ConstraintLayout

    init {
        setCardBackgroundColor(Color.WHITE)
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)

        constraintLayout = ConstraintLayout(context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        addView(constraintLayout)
    }

    fun setupTextViews(item: String) {
        constraintLayout.removeAllViews() // Clear existing views

        for (col in 0 until 5) {
            val textView = TextView(context).apply {
                text = "[$col] $item"
                gravity = Gravity.START
                setPadding(16, 16, 16, 16)
                id = View.generateViewId() // Assign a unique ID for ConstraintLayout
            }

            val layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.WRAP_CONTENT,
                ConstraintLayout.LayoutParams.WRAP_CONTENT
            )

            if (col == 0) {
                layoutParams.topToTop = ConstraintLayout.LayoutParams.PARENT_ID
                layoutParams.leftToLeft = ConstraintLayout.LayoutParams.PARENT_ID
            } else {
                layoutParams.topToTop = ConstraintLayout.LayoutParams.PARENT_ID
                layoutParams.leftToRight = constraintLayout.getChildAt(col - 1).id
                layoutParams.setMargins(16, 0, 0, 0) // Margin between columns
            }

            textView.layoutParams = layoutParams
            constraintLayout.addView(textView)
        }
    }
}
```
