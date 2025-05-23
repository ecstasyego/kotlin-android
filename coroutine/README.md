## Coroutines by Application Architecture
### Application
- Dispatchers.IO
    - `GlobalScope.launch(Dispatchers.IO){}`
    - `GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
    - `GlobalScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
    - `GlobalScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
- Dispatchers.Main
    - `GlobalScope.launch(Dispatchers.Main){}`
    - `GlobalScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
    - `GlobalScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
    - `GlobalScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
- Dispatchers.Default
    - Explicit
        - `GlobalScope.launch(Dispatchers.Default){}`
        - `GlobalScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - Implicit
          `GlobalScope.launch{}`
        - `GlobalScope.launch{withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch{async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`

```kts
GlobalScope.launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
GlobalScope.launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
GlobalScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
GlobalScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```


### ViewModel
- `viewModelScope.launch{}`
  
### Activity
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
- `lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
- `lifecycleScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`

```kts
lifecycleScope.launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
lifecycleScope.launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
lifecycleScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
lifecycleScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```


### Fragment  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
- `viewLifecycleOwner.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`

```kts
viewLifecycleOwner.launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
viewLifecycleOwner.launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
viewLifecycleOwner.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
viewLifecycleOwner.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```


### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`

```kts
CoroutineScope(Dispatchers.Default).launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
CoroutineScope(Dispatchers.Default).launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
CoroutineScope(Dispatchers.Default).launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
CoroutineScope(Dispatchers.Default).launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```


### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`





