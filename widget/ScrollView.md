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
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="첫 번째 텍스트"
            android:textSize="18sp"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="두 번째 텍스트"
            android:textSize="18sp"/>

        <Button
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="버튼"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="세 번째 텍스트"
            android:textSize="18sp"/>

    </LinearLayout>

</ScrollView>
```

![image](https://github.com/user-attachments/assets/1ec9da54-cb06-4736-80f3-59dce40d5610)
