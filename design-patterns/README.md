## Android Application Architecture
- Principle
  - Separation of Concerns
  - Modularization
  - UI-First Design
- Android Architecture Components, AAC
  - **ViewModel**: The ViewModel is responsible for preparing and managing the data for the UI. It holds UI-related data in a lifecycle-conscious way and survives configuration changes (such as screen rotations).
  - **LiveData**: LiveData is an observable data holder class that allows UI components (like activities or fragments) to observe data changes in a lifecycle-aware way. It ensures that UI components are updated with the latest data when necessary.
  - **Room**: Room is an abstraction layer over SQLite that allows for more robust database access. It simplifies database interactions and ensures that database queries are performed in a background thread, making apps more responsive.
  - **Repository**: The Repository is a pattern that acts as a clean API for data access. It abstracts the data sources (network, cache, local database) and provides a unified interface for the ViewModel to fetch data. It helps in decoupling the data layer from the rest of the app.
  - **WorkManager**: WorkManager provides a simple and flexible way to manage background tasks that need to be guaranteed, such as syncing data with a server or running periodic tasks.
  - **Navigation Component**: The Navigation component helps in managing navigation and the back stack in a modular, consistent, and predictable way. It simplifies fragment transactions and deep linking in the app.
  - **Lifecycle**: The Android Lifecycle component ensures that the app's components (such as activities and fragments) are lifecycle-aware. It helps manage the UI-related tasks, so the app responds to lifecycle changes without worrying about the details of the lifecycle.









<br><br><br>

## Design Patterns
- [Behavior]
  - Action > (View) > ...
    - ... > View

### MVC: Model + View + Controller
- [Behavior]
  - Action > (View) > Controller
    -  Model <-> Controller <-> View
- Controller : View
  - 1 : n
    - Controller : View01, View02, View03, ...
- (Dependency) **Model** : **View** 

<br>

### MVP: Model + View + Presenter
- [Behavior]
  - Action > View > Presenter > Model
    - Model > Presenter > View
- (Dependency) Presenter : View
  - 1 : 1
    - Presenter01 : View01
    - Presenter02 : View02
    - Presenter03 : View03
    - ...
- (Independency) **Model** : **View** 


<br>

### MVVM: Model + View + ViewModel
```
Model: Represents the data and business logic.
View: Represents the UI components (Activities and Fragments).
ViewModel: Acts as a bridge between the UI and the Model, exposing LiveData to the UI and handling business logic.
```

- [Behavior]
  - Action > View > ViewModel > Model
    - Model > ViewModel > View
- (Independency) ViewModel : View
  - 1 : n
    - ViewModel : View01, View02, View03, ...
- (Independency) **Model** : **View** 


