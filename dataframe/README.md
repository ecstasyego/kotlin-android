
## Examples
- https://github.com/jjoe64/GraphView/wiki/Documentation

### Example01: Basic
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
├── build.gradle.kts # PROJECT-LEVEL
└── gradle.properties # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("org.jetbrains.kotlinx:dataframe:0.8.0")
}
```





