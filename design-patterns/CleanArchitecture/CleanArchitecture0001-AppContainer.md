
## CleanArchitecture0001
### Architecture Guide
```
.Class
├── Dependency Injection
│   └── Application
├── Presentation Layer(UI Layer)
│   ├── Activity
│   ├── View
│   └── ViewModel
├── Domain Layer
│   └── UseCase
└── Data Layer
    ├── Repository
    └── APIService
```

<br><br><br>

### Example01: Basic
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.app.Application
import android.os.Bundle
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.Observer
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import kotlinx.coroutines.launch

// Dependency Injection
object AppContainer {
    private lateinit var applicationContext: Application

    fun initialize(application: Application) { applicationContext = application }
    val apiService: APIService by lazy { APIServiceImpl() }
    val userRepository: UserRepository by lazy { UserRepositoryImpl(apiService) }
    val userUseCase: UserUseCase by lazy { GetUserUseCase(userRepository) }
}


class UserApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        AppContainer.initialize(this)
    }

    override fun onTerminate() {
        super.onTerminate()
    }

}

// Presentation Layer
class MainActivity : ComponentActivity() {
    private lateinit var userViewModel: UserViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        userViewModel = UserViewModel(AppContainer.userUseCase)
        userViewModel.getUserDetails(1)
        userViewModel.userDetails.observe(this, Observer { user ->
            findViewById<TextView>(R.id.textView).text = user.toString()
        })
    }
}

class UserViewModel(private val userUseCase: UserUseCase) : ViewModel() {
    private val _userDetails = MutableLiveData<User>()
    val userDetails: LiveData<User> get() = _userDetails

    fun getUserDetails(userId: Int) {
        viewModelScope.launch {
            _userDetails.value = userUseCase.getUserDetails(userId)
        }
    }
}


// Domain Layer
class GetUserUseCase(private val userRepository: UserRepository) : UserUseCase {
    override suspend fun getUserDetails(userId: Int): User {
        return userRepository.getUserDetails(userId)
    }
}

interface UserUseCase {
    suspend fun getUserDetails(userId: Int): User
}

data class User(
    val id: Int,
    val name: String,
    val email: String
)



// Data Layer
class UserRepositoryImpl(private val apiService: APIService) : UserRepository {
    override suspend fun getUserDetails(userId: Int): User {
        val response = apiService.getUserDetails(userId)
        return User(response.id, response.name, response.email)
    }
}

interface UserRepository {
    suspend fun getUserDetails(userId: Int): User
}


class APIServiceImpl : APIService {
    override suspend fun getUserDetails(userId: Int): APIResponse {
        return APIResponse(id = userId, name = "John Doe", email = "john@example.com")
    }
}

interface APIService {
    suspend fun getUserDetails(userId: Int): APIResponse
}

data class APIResponse(
    val id: Int,
    val name: String,
    val email: String
)
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:name=".UserApplication"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>

</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</LinearLayout>
```

<br>

### Example02: Project Layering Structure
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/
│   │       │   ├── UserApplication.kt
│   │       │   ├── di
│   │       │   │   ├── AppContainer.kt
│   │       │   ├── presentation
│   │       │   │   ├── MainActivity.kt
│   │       │   │   └── UserViewModel.kt
│   │       │   ├── domain
│   │       │   │   ├── GetUserUseCase.kt
│   │       │   │   ├── UserUseCase.kt
│   │       │   │   └── User.kt
│   │       │   └── data
│   │       │       ├── UserRepositoryImpl.kt
│   │       │       ├── UserRepository.kt
│   │       │       ├── APIServiceImpl.kt
│   │       │       └── APIService.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`UserApplication.kt`
```kotlin
package com.example.myapplication

import android.app.Application
import com.example.myapplication.di.AppContainer

class UserApplication : Application() {
    override fun onCreate() {
        super.onCreate()
        AppContainer.initialize(this)
    }

    override fun onTerminate() {
        super.onTerminate()
    }
}
```

`di/AppContainer.kt`
```kotlin
package com.example.myapplication.di

import android.app.Application
import com.example.myapplication.data.APIService
import com.example.myapplication.data.APIServiceImpl
import com.example.myapplication.data.UserRepository
import com.example.myapplication.data.UserRepositoryImpl
import com.example.myapplication.domain.GetUserUseCase
import com.example.myapplication.domain.UserUseCase

// Dependency Injection
object AppContainer {
    private lateinit var applicationContext: Application

    fun initialize(application: Application) { applicationContext = application }
    val apiService: APIService by lazy { APIServiceImpl() }
    val userRepository: UserRepository by lazy { UserRepositoryImpl(apiService) }
    val userUseCase: UserUseCase by lazy { GetUserUseCase(userRepository) }
}
```

`presentation/MainActivity.kt`
```kotlin
package com.example.myapplication.presentation

import android.os.Bundle
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.lifecycle.Observer
import com.example.myapplication.R
import com.example.myapplication.di.AppContainer


// Presentation Layer
class MainActivity : ComponentActivity() {
    private lateinit var userViewModel: UserViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        userViewModel = UserViewModel(AppContainer.userUseCase)
        userViewModel.getUserDetails(1)
        userViewModel.userDetails.observe(this, Observer { user ->
            findViewById<TextView>(R.id.textView).text = user.toString()
        })
    }
}
```

`presentation/UserViewModel.kt`
```kotlin
package com.example.myapplication.presentation

import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import androidx.lifecycle.viewModelScope
import com.example.myapplication.domain.User
import com.example.myapplication.domain.UserUseCase
import kotlinx.coroutines.launch

class UserViewModel(private val userUseCase: UserUseCase) : ViewModel() {
    private val _userDetails = MutableLiveData<User>()
    val userDetails: LiveData<User> get() = _userDetails

    fun getUserDetails(userId: Int) {
        viewModelScope.launch {
            _userDetails.value = userUseCase.getUserDetails(userId)
        }
    }
}
```

`domain/GetUserUseCase.kt`
```kotlin
package com.example.myapplication.domain

import com.example.myapplication.data.UserRepository

class GetUserUseCase(private val userRepository: UserRepository) : UserUseCase {
    override suspend fun getUserDetails(userId: Int): User {
        return userRepository.getUserDetails(userId)
    }
}
```

`domain/UserUseCase.kt`
```kotlin
package com.example.myapplication.domain

interface UserUseCase {
    suspend fun getUserDetails(userId: Int): User
}
```

`domain/User.kt`
```kotlin
package com.example.myapplication.domain

data class User(
    val id: Int,
    val name: String,
    val email: String
)
```

`data/UserRepositoryImpl.kt`
```kotlin
package com.example.myapplication.data

import com.example.myapplication.domain.User

class UserRepositoryImpl(private val apiService: APIService) : UserRepository {
    override suspend fun getUserDetails(userId: Int): User {
        val response = apiService.getUserDetails(userId)
        return User(response.id, response.name, response.email)
    }
}
```

`data/UserRepository.kt`
```kotlin
package com.example.myapplication.data

import com.example.myapplication.domain.User

interface UserRepository {
    suspend fun getUserDetails(userId: Int): User
}
```

`data/APIServiceImpl.kt`
```kotlin
package com.example.myapplication.data

class APIServiceImpl : APIService {
    override suspend fun getUserDetails(userId: Int): APIResponse {
        return APIResponse(id = userId, name = "John Doe", email = "john@example.com")
    }
}
```

`data/APIService.kt`
```kotlin
package com.example.myapplication.data

interface APIService {
    suspend fun getUserDetails(userId: Int): APIResponse
}

data class APIResponse(
    val id: Int,
    val name: String,
    val email: String
)
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:name=".UserApplication"
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">

        <activity
            android:name=".presentation.MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

    </application>
</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
</LinearLayout>
```

