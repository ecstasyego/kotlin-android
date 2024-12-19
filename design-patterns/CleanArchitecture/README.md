# Clean Architecture
- Application (DEPENDENCY INJECTION)
    - Activity (PRESENTATION-LAYER)
        - View (PRESENTATION-LAYER)
            - ViewModel (PRESENTATION-LAYER)
                - **UseCase (DOMAIN-LAYER)**
                    - Repository (DOMAIN|DATA-LAYER)
                        - APIService (DATA-LAYER)

```
.Class
├── Dependency Injection
│   └── Application
├── Presentation Layer(UI Layer)
│   ├── Activity
│   ├── View
│   └── ViewModel
├── Domain Layer
│   └── UseCase
└── Data Layer
    ├── Repository
    ├── APIService
    ├── RemoteDatabase
    └── LocalDatabase
```
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication
│   │       ├── java/com/example/myapplication/
│   │       │   ├── Application.kt
│   │       │   ├── presentation
│   │       │   │   ├── Activity.kt
│   │       │   │   └── ViewModel.kt
│   │       │   ├── domain
│   │       │   │   └── UseCase.kt
│   │       │   └── data
│   │       │   │   ├── Repository.kt
│   │       │   │   └── APIService.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```




