## Examples
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.util.Log
import androidx.activity.ComponentActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import retrofit2.http.*


class MainActivity : ComponentActivity() {

    private lateinit var retrofit: Retrofit
    private lateinit var api: ApiService

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        retrofit = Retrofit.Builder().baseUrl("http://10.0.2.2:8000").addConverterFactory(GsonConverterFactory.create()).build()
        api = retrofit.create(ApiService::class.java)

        lifecycleScope.launch {
            withContext(Dispatchers.IO){
                try {
                    val getResponse = api.getData("Toy")
                    Log.d("API_GET", getResponse.toString())

                    val postResponse = api.postData(Request("Bob", 25))
                    Log.d("API_POST", postResponse.toString())

                    val putResponse = api.putData("Toy", Request("Bob", 25))
                    Log.d("API_PUT", putResponse.toString())

                    val patchResponse = api.patchData("Toy", mapOf("Bob" to 25))
                    Log.d("API_PATCH", patchResponse.toString())

                    val deleteResponse = api.deleteData("Toy")
                    Log.d("API_DELETE", deleteResponse.toString())

                } catch (e: Exception) {
                    Log.e("API_ERROR", "Network call failed", e)
                }
            }
        }
    }
}

interface ApiService {
    @GET("/src/{param00}")
    suspend fun getData(@Path("param00") param00: String): Map<String, Any>

    @POST("/src")
    suspend fun postData(@Body param00: Request): Map<String, Any>

    @PUT("/src/{param00}")
    suspend fun putData(@Path("param00") param00: String, @Body param01: Request): Map<String, Any>

    @PATCH("/src/{param00}")
    suspend fun patchData(@Path("param00") param00: String, @Body param01: Map<String, @JvmSuppressWildcards Any>): Map<String, Any>

    @DELETE("/src/{param00}")
    suspend fun deleteData(@Path("param00") param00: String): Map<String, Any>
}

data class Request(
    val rqst00: String,
    val rqst01: Int
)
```



`build.gradle.kts(APP-LEVEL)`
```kotlin
plugins {
    id("kotlin-kapt") // for Room annotation processing
}

android {
    kapt {
        correctErrorTypes = true
    }
}

dependencies {
    implementation("androidx.activity:activity-ktx:1.7.2")  // Required for activityViewModels()
    implementation("androidx.fragment:fragment-ktx:1.6.1")  // Fragment KTX for enhanced features
    implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")  // ViewModel KTX
    implementation("androidx.lifecycle:lifecycle-livedata-ktx:2.6.1")  // LiveData KTX

    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3")

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor
}
```
