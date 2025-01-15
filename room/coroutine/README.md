```
db = room()
repo = repository(db)
uc = usecase(repo)
viewmodel = viewmodelfactory(uc)
```

```
db = room()
repo = repository(db)
viewmodel = viewmodelfactory(repo)
```

```
db = room()
viewmodel = viewmodelfactory(db)
```
