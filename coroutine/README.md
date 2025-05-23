## Coroutines by Application Architecture
### Application
- `GlobalScope.launch(Dispatchers.IO){}`
- `GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `GlobalScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

### ViewModel
- `viewModelScope.launch{}`
  
### Activity
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

### Fragment  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main) {}; }`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){async(Dispatchers.Main) {}.await(); }`

### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`





