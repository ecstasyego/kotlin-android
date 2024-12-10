## LinearLayout

https://developer.android.com/reference/android/widget/LinearLayout

### XML attributes
- android:baselineAligned : When set to false, prevents the layout from aligning its children's baselines. 
- android:baselineAlignedChildIndex : When a linear layout is part of another layout that is baseline aligned, it can specify which of its children to baseline align to (that is, which child TextView). 
- android:divider : Drawable to use as a vertical divider between buttons. 
- android:gravity : Specifies how an object should position its content, on both the X and Y axes, within its own bounds. 
- android:measureWithLargestChild : When set to true, all children with a weight will be considered having the minimum size of the largest child. 
- android:orientation : Should the layout be a column or a row? Use "horizontal" for a row, "vertical" for a column. 
- android:weightSum : Defines the maximum weight sum. 


### Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!" />

</LinearLayout>
```

![image](https://github.com/user-attachments/assets/b7c35f7c-e6c6-4981-8eb5-315740872c66)


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

</LinearLayout>
```

![image](https://github.com/user-attachments/assets/f0a2f228-44d7-4146-bfa5-34ad0c7532e8)


