## Coroutines by Application Architecture with Room
### Application
- GlobalScope.launch(Dispatchers.IO){}
- GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}

### ViewModel
- `viewModelScope.launch{}`
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
  
### Repository, Database
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`
```
Activity
  db = room()
  repo = repository(db)
```
```
Activity
  db = room()
```
  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`
```
Fragment
  db = room()
  repo = repository(db)
```
```
Fragment
  db = room()
```

### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`


### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`




