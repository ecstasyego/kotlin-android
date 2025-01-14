# Clean Architecture
- Application (DEPENDENCY INJECTION-LAYER)
    - Activity (PRESENTATION-LAYER)
        - View (PRESENTATION-LAYER)
            - ViewModel (PRESENTATION-LAYER)
                - **UseCase (DOMAIN-LAYER)**
                    - Repository (DATA-LAYER)
                        - APIService (DATA-LAYER)

```
.Class
├── Dependency Injection Layer
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
│   │       │   ├── Application.kt
│   │       │   ├── presentation
│   │       │   │   ├── Activity.kt
│   │       │   │   ├── Fragment.kt
│   │       │   │   └── ViewModel.kt
│   │       │   ├── domain
│   │       │   │   └── UseCase.kt
│   │       │   └── data
│   │       │       ├── entity
│   │       │       │   └── Entity.kt
│   │       │       ├── dao
│   │       │       │   └── Dao.kt
│   │       │       ├── db
│   │       │       │   └── DB.kt
│   │       │       ├── repository
│   │       │       │   └── Repository.kt
│   │       │       ├── api
│   │       │       │   └── API.kt
│   │       │       └── network
│   │       │           └── Service.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```




