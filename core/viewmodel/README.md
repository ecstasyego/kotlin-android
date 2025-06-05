
## Definition
```kts
import androidx.lifecycle.ViewModel
import androidx.lifecycle.ViewModelProvider
import androidx.activity.viewModels
import androidx.fragment.app.activityViewModels

class MainActivity : AppCompatActivity() {
    private val viewModel = MainViewModel() // No Lifecycle Management
    private val viewModel = ViewModelProvider(this).get(MainViewModel::class.java) // Lifecycle Awareness
    private val viewModel = ViewModelProvider(this, MainViewModelFactory( widget )).get(MainViewModel::class.java) // Lifecycle Awareness

    private val viewModel: MainViewModel by lazy { MainViewModel() } // No Lifecycle Management
    private val viewModel: MainViewModel by lazy { MainViewModel( widget ) } // No Lifecycle Management
    private val viewModel: MainViewModel by lazy { ViewModelProvider(this, MainViewModelFactory( widget )).get(MainViewModel::class.java) } // Lifecycle Awareness

    private val viewModel: MainViewModel by viewModels() // Lifecycle Awareness
    private val viewModel: MainViewModel by viewModels { MainViewModelFactory( widget ) } // Lifecycle Awareness
}

class MainFragment : Fragment() {
    private val viewModel = MainViewModel() // No Lifecycle Management
    private val viewModel = ViewModelProvider(this).get(MainViewModel::class.java) // Lifecycle Awareness
    private val viewModel = ViewModelProvider(this, MainViewModelFactory( widget )).get(MainViewModel::class.java) // Lifecycle Awareness

    private val viewModel: MainViewModel by lazy { MainViewModel() } // No Lifecycle Management
    private val viewModel: MainViewModel by lazy { MainViewModel( widget ) } // No Lifecycle Management
    private val viewModel: MainViewModel by lazy { ViewModelProvider(this, MainViewModelFactory( widget )).get(MainViewModel::class.java) } // Lifecycle Awareness

    private val viewModel: MainViewModel by viewModels() // Lifecycle Awareness
    private val viewModel: MainViewModel by viewModels { MainViewModelFactory( widget ) } // Lifecycle Awareness

    private val viewModel: MainViewModel by activityViewModels() // Lifecycle Awareness
    private val viewModel: MainViewModel by activityViewModels{ MainViewModelFactory( widget ) } // Lifecycle Awareness
}
```


