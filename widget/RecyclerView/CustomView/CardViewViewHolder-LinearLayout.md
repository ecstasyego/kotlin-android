

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
import android.widget.LinearLayout
import android.view.Gravity
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

    class MyViewHolder(val customCardView: CustomCardView) : RecyclerView.ViewHolder(customCardView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val customCardView = CustomCardView(parent.context).apply {
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
            addLinearLayoutWithTextViews(5) // Add 10 columns of TextViews
        }

        return MyViewHolder(customCardView)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        val linearLayout = holder.customCardView.getChildAt(0) as LinearLayout
        for (col in 0 until linearLayout.childCount) {
            val textView = linearLayout.getChildAt(col) as TextView
            textView.text = "[$col] ${items[position]}"
        }
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

    init {
        setCardBackgroundColor(Color.WHITE)
        radius = 12f
        setCardElevation(4f)
        setContentPadding(16, 16, 16, 16)
    }

    fun addLinearLayoutWithTextViews(columns: Int) {
        val linearLayout = LinearLayout(context).apply {
            orientation = LinearLayout.HORIZONTAL
            layoutParams = LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.WRAP_CONTENT)
        }

        for (col in 0 until columns) {
            val textView = TextView(context).apply {
                text = "[$col]"
                gravity = Gravity.START
                setPadding(16, 16, 16, 16)
                layoutParams = LayoutParams(0, LayoutParams.WRAP_CONTENT, 1f)
            }
            linearLayout.addView(textView)
        }

        addView(linearLayout)
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
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
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

    class MyViewHolder(val itemView: View) : RecyclerView.ViewHolder(itemView)

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        // Inflate the XML layout for each item (item_custom_card.xml)
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_card, parent, false)
        return MyViewHolder(view)
    }

    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        // Get references to the TextViews from the inflated layout
        val textView1 = holder.itemView.findViewById<TextView>(R.id.textView1)
        val textView2 = holder.itemView.findViewById<TextView>(R.id.textView2)
        val textView3 = holder.itemView.findViewById<TextView>(R.id.textView3)
        val textView4 = holder.itemView.findViewById<TextView>(R.id.textView4)
        val textView5 = holder.itemView.findViewById<TextView>(R.id.textView5)

        // Bind the data to the TextViews
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
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardBackgroundColor="@android:color/white"
    app:cardCornerRadius="12dp"
    app:cardElevation="4dp"
    android:layout_margin="16dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="16dp">

        <TextView
            android:id="@+id/textView1"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="[0]"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            android:layout_weight="1"/>

        <TextView
            android:id="@+id/textView2"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="[1]"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            android:layout_weight="1"/>

        <TextView
            android:id="@+id/textView3"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="[2]"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            android:layout_weight="1"/>

        <TextView
            android:id="@+id/textView4"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="[3]"
            android:gravity="start"
            android:layout_marginEnd="16dp"
            android:layout_weight="1"/>

        <TextView
            android:id="@+id/textView5"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="[4]"
            android:gravity="start"
            android:layout_weight="1"/>

    </LinearLayout>
</androidx.cardview.widget.CardView>
```
