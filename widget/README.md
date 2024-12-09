## Activity
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.widget.*
import android.content.Intent

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout) // res/layout/main_layout.xml

        val widget = findViewById<[InputWidgetClass]>(R.id.[WidgetID]) // *.xml
        widget.setClickable(true)
        widget.setOnClickListener{
            val intent = Intent(this, ResultActivity::class.java)
            intent.putExtra("height", 50.0)
            intent.putExtra("width", 200.0)
            startActivity(intent)
        }
    }
}

class ResultActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_result) // res/layout/activity_result.xml

        val height = intent.getIntExtra("height", 0)
        val width = intent.getIntExtra("width", 0)
        Log.d("ResultActivity", "height : $height , width : $width")

        val widget = findViewById<[OutputWidgetClass]>(R.id.[WidgetID])
        widget.text = height * width  
    }
}
```

```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.widget.*
import android.content.Intent

class MainActivity : ComponentActivity() {
    private val widget: [InputWidgetClass] by lazy { findViewById<[InputWidgetClass]>(R.id.[WidgetID]) }  // *.xml

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout) // res/layout/main_layout.xml
        [inputWidget]()
        }

    private fun [inputWidget]() {
        widget.setClickable(true)
        widget.setOnClickListener{
            val intent = Intent(this, ResultActivity::class.java)
            intent.putExtra("height", 50.0)
            intent.putExtra("width", 200.0)
            startActivity(intent)
        }
    }
}

class ResultActivity: AppCompatActivity() {
    private val widget:[OutputWidgetClass] by lazy { findViewById<[OutputWidgetClass]>(R.id.[WidgetID]) }  // *.xml

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_result) // res/layout/activity_result.xml

        val height = intent.getIntExtra("height", 0)
        val width = intent.getIntExtra("width", 0)
        Log.d("ResultActivity", "height : $height , width : $width")

        widget.text = height * width  
    }
}
```


<br><br><br>
## Tools
View > Tool Windows > Layout Validation
![image](https://github.com/user-attachments/assets/f616a8ff-dc9b-4bad-ad8d-e1f3782fc288)


## API Reference

### Layout
- https://developer.android.com/reference/android/widget/LinearLayout
    - findViewById<LinearLayout>(R.id.[LayoutID]).addView()
    - findViewById<LinearLayout>(R.id.[LayoutID]).removeView()
- https://developer.android.com/reference/android/widget/FrameLayout
- https://developer.android.com/reference/android/widget/TableLayout
- https://developer.android.com/reference/androidx/constraintlayout/widget/ConstraintLayout
- https://developer.android.com/reference/androidx/swiperefreshlayout/widget/SwipeRefreshLayout
- https://developer.android.com/reference/androidx/coordinatorlayout/widget/CoordinatorLayout
- https://developer.android.com/reference/com/google/android/material/appbar/CollapsingToolbarLayout
- https://developer.android.com/reference/com/google/android/material/appbar/AppBarLayout

### Input
- https://developer.android.com/reference/android/widget/Button
    - Button().setText()
    - Button().setOnClickListener()
    - Button().setEnabled()
- https://developer.android.com/reference/android/widget/CheckBox
    - CheckBox().setChecked()
    - CheckBox().isChecked()
- https://developer.android.com/reference/android/widget/EditText
    - EditText().setText()
    - EditText().getText()
    - EditText().setHint()
    - EditText().setSelection()
- https://developer.android.com/reference/android/widget/RadioButton
    - RadioButton().setChecked()
    - RadioButton().isChecked()
- https://developer.android.com/reference/android/widget/RadioGroup
- https://developer.android.com/reference/android/widget/Spinner
    - Spinner().setSelection()
    - Spinner().getSelectedItem()
- https://developer.android.com/reference/android/widget/AutoCompleteTextView
- https://developer.android.com/reference/android/widget/MultiAutoCompleteTextView
- https://developer.android.com/reference/android/widget/SeekBar
    - SeekBar().setProgress()
    - SeekBar().getProgress()
    - SeekBar().setMax() 
- https://developer.android.com/reference/android/widget/RatingBar
- https://developer.android.com/reference/android/widget/DatePicker
- https://developer.android.com/reference/android/widget/TimePicker
- https://developer.android.com/reference/android/widget/NumberPicker
- https://developer.android.com/reference/android/widget/Switch
- https://developer.android.com/reference/android/widget/ToggleButton

### Output(View)
- https://developer.android.com/reference/android/widget/ScrollView
- https://developer.android.com/reference/android/widget/TextView
    - TextView().setText()
    - TextView().setTextColor()
    - TextView().setTextSize()
    - TextView().setVisibility()
- https://developer.android.com/reference/android/widget/ImageView
    - ImageView().setImageResource()
    - ImageView().setImageBitmap()
    - ImageView().setImageURI()
- https://developer.android.com/reference/androidx/cardview/widget/CardView
- https://developer.android.com/reference/androidx/core/widget/NestedScrollView
- https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView
- https://developer.android.com/reference/android/widget/ProgressBar
    - ProgressBar().setProgress()
    - ProgressBar().setMax()

