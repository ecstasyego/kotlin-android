# Design Patterns
- Action > (View) > ...
  - ... > View

## MVC: Model + View + Controller
- Action > (View) > Controller
  -  Model <-> Controller <-> View
- Controller : View
  - 1 : n
    - Controller : View01, View02, View03, ...

<br>

## MVP: Model + View + Presenter
- Action > View > Presenter > Model
  - Model > Presenter > View
- Presenter : View
  - 1 : 1
    - Presenter01 : View01
    - Presenter02 : View02
    - Presenter03 : View03
    - ...


<br>

## MVVM: Model + View + ViewModel
- Action > View > ViewModel > Model
  - Model > ViewModel > View
- ViewModel : View
  - 1 : n
    - ViewModel : View01, View02, View03, ...

