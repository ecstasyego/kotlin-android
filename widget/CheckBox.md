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

    <CheckBox
        android:id="@+id/myCheckBox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Accept Terms and Conditions"
        android:layout_marginTop="20dp"/>

</LinearLayout>
```

![image](https://github.com/user-attachments/assets/5b7ef44f-7c02-496c-b416-d9f6127923a2)
