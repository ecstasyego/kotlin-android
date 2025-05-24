## Process
### Process State
![ProcessState](https://github.com/user-attachments/assets/37e0c347-fcfe-4e4c-840d-1977127d2976)

- new(create)
- running
- ready
- waiting
- terminated


## Thread

## Coroutine


<br><br><br>

---

## Coroutines by Application Architecture
### Application: GlobalScope
- Dispatchers.IO
    - **GlobalScope.launch(Dispatchers.IO) { ... }**
        - `GlobalScope.launch(Dispatchers.IO){}`
        - `GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `GlobalScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `GlobalScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
        - `GlobalScope.launch(Dispatchers.IO){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Main
    - **GlobalScope.launch(Dispatchers.Main) { ... }**
        - `GlobalScope.launch(Dispatchers.Main){}`
        - `GlobalScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `GlobalScope.launch(Dispatchers.Main){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Default
    - **GlobalScope.launch(Dispatchers.Default) { ... }**
        - `GlobalScope.launch(Dispatchers.Default){}`
        - `GlobalScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `GlobalScope.launch(Dispatchers.Default){joinAll( launch{}, launch{}, ... ); }`
    - **GlobalScope.launch { ... }**
        - `GlobalScope.launch{}`
        - `GlobalScope.launch{withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch{async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `GlobalScope.launch{joinAll( launch{}, launch{}, ... ); }`

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

```kts
GlobalScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    task1.join()
    task2.join()

    postprocessing()
}
```

```kts
GlobalScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    joinAll(task1, task2)

    postprocessing()
}
```


### ViewModel
- `viewModelScope.launch{}`
  
### Activity: lifecycleScope
- Dispatchers.IO
    - **lifecycleScope.launch(Dispatchers.IO) { ... }**
        - `lifecycleScope.launch(Dispatchers.IO){}`
        - `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
        - `lifecycleScope.launch(Dispatchers.IO){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Main
    - **lifecycleScope.launch(Dispatchers.Main) { ... }**
        - `lifecycleScope.launch(Dispatchers.Main){}`
        - `lifecycleScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `lifecycleScope.launch(Dispatchers.Main){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Default
    - **lifecycleScope.launch(Dispatchers.Default) { ... }**
        - `lifecycleScope.launch(Dispatchers.Default){}`
        - `lifecycleScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `lifecycleScope.launch(Dispatchers.Default){joinAll( launch{}, launch{}, ... ); }`
    - **lifecycleScope.launch { ... }**
        - `lifecycleScope.launch{}`
        - `lifecycleScope.launch{withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch{async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `lifecycleScope.launch{joinAll( launch{}, launch{}, ... ); }`

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

```kts
lifecycleScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    task1.join()
    task2.join()

    postprocessing()
}
```

```kts
lifecycleScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    joinAll(task1, task2)

    postprocessing()
}
```


### Fragment: viewLifecycleOwner.lifecycleScope
- Dispatchers.IO
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Main
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Default
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){joinAll( launch{}, launch{}, ... ); }`
    - **viewLifecycleOwner.lifecycleScope.launch { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch{}`
        - `viewLifecycleOwner.lifecycleScope.launch{withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch{async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `viewLifecycleOwner.lifecycleScope.launch{joinAll( launch{}, launch{}, ... ); }`

```kts
viewLifecycleOwner.lifecycleScope.launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
viewLifecycleOwner.lifecycleScope.launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
viewLifecycleOwner.lifecycleScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
viewLifecycleOwner.lifecycleScope.launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```

```kts
viewLifecycleOwner.lifecycleScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    task1.join()
    task2.join()

    postprocessing()
}
```

```kts
viewLifecycleOwner.lifecycleScope.launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    joinAll(task1, task2)

    postprocessing()
}
```


### Service
- Dispatchers.IO
    - **CoroutineScope(Dispatchers.IO).launch { ... }**
        - `CoroutineScope(Dispatchers.IO).launch{}`
        - `CoroutineScope(Dispatchers.IO).launch{withContext(Dispatchers.Main){}; }`
        - `CoroutineScope(Dispatchers.IO).launch{async(Dispatchers.Main){}.await(); }`
        - `CoroutineScope(Dispatchers.IO).launch{awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
        - `CoroutineScope(Dispatchers.IO).launch{joinAll( launch{}, launch{}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Main
    - **CoroutineScope(Dispatchers.Main).launch { ... }**
        - `CoroutineScope(Dispatchers.Main).launch{}`
        - `CoroutineScope(Dispatchers.Main).launch{withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Main).launch{async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Main).launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `CoroutineScope(Dispatchers.Main).launch{joinAll( launch{}, launch{}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){joinAll( launch{}, launch{}, ... ); }`
- Dispatchers.Default
    - **CoroutineScope(Dispatchers.Default).launch { ... }**
        - `CoroutineScope(Dispatchers.Default).launch{}`
        - `CoroutineScope(Dispatchers.Default).launch{withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch{async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `CoroutineScope(Dispatchers.Default).launch{joinAll( launch{}, launch{}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){joinAll( launch{}, launch{}, ... ); }`

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

```kts
CoroutineScope(Dispatchers.Default).launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    task1.join()
    task2.join()

    postprocessing()
}
```

```kts
CoroutineScope(Dispatchers.Default).launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    joinAll(task1, task2)

    postprocessing()
}
```


<br><br><br>

---


## Coroutine & Thread
### Coroutine: launch
- `launch{}`
- **launch() { ... }**
    - `launch{}`
    - `launch(Dispatchers.Default){}`
    - `launch(Dispatchers.Main){}`
    - `launch(Dispatchers.IO){}`
- **[SCOPE].launch { ... }**
    - `runBlocking{launch{}}`
    - `GlobalScope.launch{}`
    - `viewModelScope.launch{}`
    - `lifecycleScope.launch{}`
    - `viewLifecycleOwner.lifecycleScope.launch{}`
    - `CoroutineScope(Dispatchers.Default).launch{}`
    - `CoroutineScope(Dispatchers.Main).launch{}`
    - `CoroutineScope(Dispatchers.IO).launch{}`
- **[SCOPE].launch() { ... }**
    - `GlobalScope.launch{}`
    - `GlobalScope.launch(Dispatchers.Default){}`
    - `GlobalScope.launch(Dispatchers.Main){}`
    - `GlobalScope.launch(Dispatchers.IO){}`
    - `viewModelScope.launch{}`
    - `viewModelScope.launch(Dispatchers.Default){}`
    - `viewModelScope.launch(Dispatchers.Main){}`
    - `viewModelScope.launch(Dispatchers.IO){}`
    - `lifecycleScope.launch{}`
    - `lifecycleScope.launch(Dispatchers.Default){}`
    - `lifecycleScope.launch(Dispatchers.Main){}`
    - `lifecycleScope.launch(Dispatchers.IO){}`
    - `viewLifecycleOwner.lifecycleScope.launch{}`
    - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){}`
    - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){}`
    - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
    - `CoroutineScope(Dispatchers.Default).launch{}`
    - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){}`
    - `CoroutineScope(Dispatchers.Main).launch(Dispatchers.Main){}`
    - `CoroutineScope(Dispatchers.IO).launch(Dispatchers.IO){}`

```kts
launch {
    withContext(Dispatchers.IO) {
        preprocessing()
    }

    postprocessing()
}
```

```kts
launch {
    async(Dispatchers.IO) {
        preprocessing()
    }.await()

    postprocessing()
}
```

```kts
launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    task1.await()
    task2.await()

    postprocessing()
}
```

```kts
launch {
    val task1 = async(Dispatchers.IO) { preprocessing1() }
    val task2 = async(Dispatchers.IO) { preprocessing2() }
    awaitAll(task1, task2)

    postprocessing()
}
```

```kts
launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    task1.join()
    task2.join()

    postprocessing()
}
```

```kts
launch {
    val task1 = launch { preprocessing1() }
    val task2 = launch { preprocessing2() }
    joinAll(task1, task2)

    postprocessing()
}
```


### Thread: Thread
- `Thread{}`
- `Thread(Runnable{})`
- `Thread(Runnable{runOnUiThread{}})`

```kts
val task1 = Thread{ preprocessing1() }
val task2 = Thread{ preprocessing2() }
task1.start()
task2.start()
task1.join()
task2.join()
```

```kts
val job = Thread{
    val task1 = Thread{ preprocessing1() }
    val task2 = Thread{ preprocessing1() }
    task1.start()
    task2.start()
    task1.join()
    task2.join()
}
job.start()
job.join()
```


