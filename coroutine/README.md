## Coroutines by Application Architecture
### Application
- `GlobalScope.launch(Dispatchers.IO){}`
- `GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `GlobalScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

```kts
GlobalScope.launch {
    val deferred1 = async(Dispatchers.IO) { task1() }
    val deferred2 = async(Dispatchers.IO) { task2() }

    val result1 = deferred1.await()
    val result2 = deferred2.await()

    updateUI(result1, result2)
}
```

### ViewModel
- `viewModelScope.launch{}`
  
### Activity
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

```kts
lifecycleScope.launch {
    val deferred1 = async(Dispatchers.IO) { task1() }
    val deferred2 = async(Dispatchers.IO) { task2() }

    val result1 = deferred1.await()
    val result2 = deferred2.await()

    updateUI(result1, result2)
}
```

### Fragment  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

```kts
viewLifecycleOwner.launch {
    val deferred1 = async(Dispatchers.IO) { task1() }
    val deferred2 = async(Dispatchers.IO) { task2() }

    val result1 = deferred1.await()
    val result2 = deferred2.await()

    updateUI(result1, result2)
}
```

### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

```kts
CoroutineScope(Dispatchers.Default).launch {
    val deferred1 = async(Dispatchers.IO) { task1() }
    val deferred2 = async(Dispatchers.IO) { task2() }

    val result1 = deferred1.await()
    val result2 = deferred2.await()

    updateUI(result1, result2)
}
```

### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`





