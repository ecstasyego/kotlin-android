# Clean Architecture
```
.
├── Dependency Injection
│   └── Application
├── Presentation Layer
│   ├── Activity
│   ├── View
│   └── ViewModel
├── Domain Layer
│   └── UseCase
└── Data Layer
    └── Repository
```
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication
│   │       │   └── MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```




- Application
  - Activity
    - View
      - ViewModel
        - UseCase
