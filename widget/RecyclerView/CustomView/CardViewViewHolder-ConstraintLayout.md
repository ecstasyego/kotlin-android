

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



<br>


### Example02: *.xml
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/item_card.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.LayoutInflater
import android.view.ViewGroup
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.cardview.widget.CardView
import androidx.constraintlayout.widget.ConstraintLayout
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

    class MyViewHolder(val cardView: CardView) : RecyclerView.ViewHolder(cardView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_card, parent, false)
        return MyViewHolder(view as CardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        val cardView = holder.cardView
        val constraintLayout = cardView.findViewById<ConstraintLayout>(R.id.constraintLayout)

        val textView1 = constraintLayout.findViewById<TextView>(R.id.textView1)
        val textView2 = constraintLayout.findViewById<TextView>(R.id.textView2)
        val textView3 = constraintLayout.findViewById<TextView>(R.id.textView3)
        val textView4 = constraintLayout.findViewById<TextView>(R.id.textView4)
        val textView5 = constraintLayout.findViewById<TextView>(R.id.textView5)

        textView1.text = "[0] ${items[position]}"
        textView2.text = "[1] ${items[position]}"
        textView3.text = "[2] ${items[position]}"
        textView4.text = "[3] ${items[position]}"
        textView5.text = "[4] ${items[position]}"
    }

    override fun getItemCount(): Int {
        return items.size
    }
}
```


`item_card.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardBackgroundColor="@android:color/white"
    app:cardCornerRadius="12dp"
    app:cardElevation="4dp"
    android:layout_margin="16dp"
    tools:context=".MainActivity">

    <androidx.constraintlayout.widget.ConstraintLayout
        android:id="@+id/constraintLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:padding="16dp">

        <TextView
            android:id="@+id/textView1"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Text 1"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Text 2"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            app:layout_constraintStart_toEndOf="@id/textView1"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/textView3"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Text 3"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            app:layout_constraintStart_toEndOf="@id/textView2"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/textView4"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Text 4"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            app:layout_constraintStart_toEndOf="@id/textView3"
            app:layout_constraintTop_toTopOf="parent" />

        <TextView
            android:id="@+id/textView5"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Text 5"
            android:gravity="start"
            app:layout_constraintStart_toEndOf="@id/textView4"
            app:layout_constraintTop_toTopOf="parent" />

    </androidx.constraintlayout.widget.ConstraintLayout>
</androidx.cardview.widget.CardView>
```



