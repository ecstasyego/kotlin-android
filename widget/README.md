## MainActivity.kt
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.CheckBox
import android.widget.DatePicker
import android.widget.EditText
import android.widget.GridLayout
import android.widget.HorizontalScrollView
import android.widget.ImageView
import android.widget.LinearLayout
import android.widget.NumberPicker
import android.widget.ProgressBar
import android.widget.RadioButton
import android.widget.RatingBar
import android.widget.ScrollView
import android.widget.SeekBar
import android.widget.Spinner
import android.widget.Switch
import android.widget.TableLayout
import android.widget.TextView
import android.widget.TimePicker
import android.widget.ToggleButton
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    lateinit var widget: View

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        // Layouts
        widget = LinearLayout(this)
        widget = TableLayout(this)
        widget = GridLayout(this)

        // Inputs
        widget = Button(this)
        widget = CheckBox(this)
        widget = EditText(this)
        widget = RadioButton(this)
        widget = Spinner(this)
        widget = SeekBar(this)
        widget = RatingBar(this)
        widget = DatePicker(this)
        widget = TimePicker(this)
        widget = NumberPicker(this)
        widget = Switch(this)
        widget = ToggleButton(this)

        // Outputs
        widget = HorizontalScrollView(this)
        widget = ScrollView(this)
        widget = TextView(this)
        widget = ImageView(this)
        widget = ProgressBar(this)

        setContentView(widget)
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

