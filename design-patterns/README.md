# Design Patterns
- [Behavior]
  - Action > (View) > ...
    - ... > View

## MVC: Model + View + Controller
- [Behavior]
  - Action > (View) > Controller
    -  Model <-> Controller <-> View
- Controller : View
  - 1 : n
    - Controller : View01, View02, View03, ...
- (Dependency) **Model** : **View** 

<br>

## MVP: Model + View + Presenter
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

## MVVM: Model + View + ViewModel
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


