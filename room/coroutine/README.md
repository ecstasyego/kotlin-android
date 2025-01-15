## Coroutines by Application Architecture with Room

### ViewModel
`viewModelScope.launch{}`
```
Activity, Fragment
  db = room()
  repo = repository(db)
  uc = usecase(repo)
  viewmodel = viewmodelfactory(uc)
```

```
Activity, Fragment
  db = room()
  repo = repository(db)
  viewmodel = viewmodelfactory(repo)
```

```
Activity, Fragment
  db = room()
  viewmodel = viewmodelfactory(db)
```

### Repository

``
```
Activity, Fragment
  db = room()
  repo = repository(db)
```



### Database

