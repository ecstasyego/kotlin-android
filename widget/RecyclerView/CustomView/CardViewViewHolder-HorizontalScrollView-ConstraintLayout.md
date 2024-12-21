
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
import android.view.Gravity
import android.widget.HorizontalScrollView
import android.widget.TextView
import androidx.cardview.widget.CardView
import androidx.constraintlayout.widget.ConstraintLayout
import android.view.ViewGroup
import android.os.Bundle
import android.view.View
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

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
        // Create CustomCardView and return MyViewHolder
        val customCardView = CustomCardView(parent.context).apply {
            layoutParams = ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT)
        }
        return MyViewHolder(customCardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        // Bind data to the CustomCardView
        holder.customCardView.setupTextViews(items[position]) // Set the 5 columns for each item
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
    private val constraintLayout: ConstraintLayout

    init {
        // Set up the CardView properties
        setCardBackgroundColor(Color.WHITE)
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)

        // Initialize the HorizontalScrollView
        horizontalScrollView = HorizontalScrollView(context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        // Initialize the ConstraintLayout inside the HorizontalScrollView
        constraintLayout = ConstraintLayout(context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        // Add the ConstraintLayout to the HorizontalScrollView
        horizontalScrollView.addView(constraintLayout)

        // Add the HorizontalScrollView to the CardView
        addView(horizontalScrollView)
    }

    // Function to add 5 TextViews to the ConstraintLayout
    fun setupTextViews(item: String) {
        constraintLayout.removeAllViews() // Clear existing views

        // Create and add 5 TextViews to the ConstraintLayout
        for (col in 0 until 5) {
            val textView = TextView(context).apply {
                text = "[$col] $item"
                gravity = Gravity.START
                setPadding(16, 16, 16, 16)
                id = View.generateViewId() // Assign a unique ID for ConstraintLayout
            }

            // Set up the layout params for each TextView
            val layoutParams = ConstraintLayout.LayoutParams(
                ConstraintLayout.LayoutParams.WRAP_CONTENT,
                ConstraintLayout.LayoutParams.WRAP_CONTENT
            )

            // Position TextViews in a horizontal row with margins
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

