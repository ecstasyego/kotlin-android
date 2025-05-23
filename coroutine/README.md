## Coroutines by Application Architecture
### Application
- GlobalScope.launch(Dispatchers.IO){}
- GlobalScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}

### ViewModel
- `viewModelScope.launch{}`
  
### Activity
- `lifecycleScope.launch(Dispatchers.IO){}`
- `lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`

### Fragment  
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){}`
- `viewLifecycleOwner.lifecycleScope.launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`

### Service
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){}`
- `CoroutineScope(Dispatchers.Default).launch(Dispatchers.IO){withContext(Dispatchers.Main) {}}`

### Thread
- `Thread{Runnable{}}`
- `Thread{Runnable{runOnUiThread{}}}`





