## Coroutines by Application Architecture with Room

### ViewModel
```
Activity, Fragment
  db = room()
  repo = repository(db)
  uc = usecase(repo)
  viewmodel = viewmodelfactory(uc)
    viewModelScope.launch{}
```

```
Activity, Fragment
  db = room()
  repo = repository(db)
  viewmodel = viewmodelfactory(repo)
    viewModelScope.launch{}
```

```
Activity, Fragment
  db = room()
  viewmodel = viewmodelfactory(db)
    viewModelScope.launch{}
```

### Repository

```
Activity, Fragment
  db = room()
  repo = repository(db)
    viewModelScope.launch{}
```



### Database

