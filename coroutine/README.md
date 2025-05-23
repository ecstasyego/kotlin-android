## Coroutines by Application Architecture
### Application: GlobalScope
- Dispatchers.IO
    - **GlobalScope.launch(Dispatchers.IO) { ... }**
        - `GlobalScope.launch(Dispatchers.IO){}`
        - `GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `GlobalScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `GlobalScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
- Dispatchers.Main
    - **GlobalScope.launch(Dispatchers.Main) { ... }**
        - `GlobalScope.launch(Dispatchers.Main){}`
        - `GlobalScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
- Dispatchers.Default
    - **GlobalScope.launch(Dispatchers.Default) { ... }**
        - `GlobalScope.launch(Dispatchers.Default){}`
        - `GlobalScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `GlobalScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `GlobalScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - **GlobalScope.launch { ... }**
        - `GlobalScope.launch{}`
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
  
### Activity: lifecycleScope
- Dispatchers.IO
    - **lifecycleScope.launch(Dispatchers.IO) { ... }**
        - `lifecycleScope.launch(Dispatchers.IO){}`
        - `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
- Dispatchers.Main
    - **lifecycleScope.launch(Dispatchers.Main) { ... }**
        - `lifecycleScope.launch(Dispatchers.Main){}`
        - `lifecycleScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
- Dispatchers.Default
    - **lifecycleScope.launch(Dispatchers.Default) { ... }**
        - `lifecycleScope.launch(Dispatchers.Default){}`
        - `lifecycleScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - **lifecycleScope.launch { ... }**
        - `lifecycleScope.launch{}`
        - `lifecycleScope.launch{withContext(Dispatchers.IO){}; }`
        - `lifecycleScope.launch{async(Dispatchers.IO){}.await(); }`
        - `lifecycleScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`

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


### Fragment: viewLifecycleOwner.lifecycleScope
- Dispatchers.IO
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
- Dispatchers.Main
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
- Dispatchers.Default
    - **viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default) { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){}`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - **viewLifecycleOwner.lifecycleScope.launch { ... }**
        - `viewLifecycleOwner.lifecycleScope.launch{}`
        - `viewLifecycleOwner.lifecycleScope.launch{withContext(Dispatchers.IO){}; }`
        - `viewLifecycleOwner.lifecycleScope.launch{async(Dispatchers.IO){}.await(); }`
        - `viewLifecycleOwner.lifecycleScope.launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`

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


### Service
- Dispatchers.IO
    - **CoroutineScope(Dispatchers.IO).launch { ... }**
        - `CoroutineScope(Dispatchers.IO).launch{}`
        - `CoroutineScope(Dispatchers.IO).launch{withContext(Dispatchers.Main){}; }`
        - `CoroutineScope(Dispatchers.IO).launch{async(Dispatchers.Main){}.await(); }`
        - `CoroutineScope(Dispatchers.IO).launch{awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){async(Dispatchers.Main){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){awaitAll( async(Dispatchers.Main){}, async(Dispatchers.Main){}, ... ); }`
- Dispatchers.Main
    - **CoroutineScope(Dispatchers.Main).launch { ... }**
        - `CoroutineScope(Dispatchers.Main).launch{}`
        - `CoroutineScope(Dispatchers.Main).launch{withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Main).launch{async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Main).launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Main){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
- Dispatchers.Default
    - **CoroutineScope(Dispatchers.Default).launch { ... }**
        - `CoroutineScope(Dispatchers.Default).launch{}`
        - `CoroutineScope(Dispatchers.Default).launch{withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch{async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch{awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`
    - **CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default) { ... }**
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){}`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){withContext(Dispatchers.IO){}; }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){async(Dispatchers.IO){}.await(); }`
        - `CoroutineScope(Dispatchers.Default).launch(Dispatchers.Default){awaitAll( async(Dispatchers.IO){}, async(Dispatchers.IO){}, ... ); }`

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





