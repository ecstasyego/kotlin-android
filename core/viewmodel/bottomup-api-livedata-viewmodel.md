package com.example.myapplication

import android.os.Bundle
import android.util.Log
import androidx.activity.ComponentActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.launch
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import retrofit2.http.*


class MainActivity : ComponentActivity() {

    private val retrofit = Retrofit.Builder().baseUrl("http://10.0.2.2:8000").addConverterFactory(GsonConverterFactory.create()).build()
    private val api = retrofit.create(ApiService::class.java)

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        lifecycleScope.launch {
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

interface ApiService {
    @GET("/src/{param00}")
    suspend fun getData(@Path("param00") param00: String): Map<String, Any>

    @POST("/src")
    suspend fun postData(@Body param00: Request): Map<String, Any>

    @PUT("/src/{param00}")
    suspend fun putData(@Path("param00") param00: String, @Body param01: Request): Map<String, Any>

    @PATCH("/src/{param00}")
    suspend fun patchData(
        @Path("param00") param00: String,
        @Body param01: Map<String, @JvmSuppressWildcards Any>
    ): Map<String, Any>

    @DELETE("/src/{param00}")
    suspend fun deleteData(@Path("param00") param00: String): Map<String, Any>
}

data class Request(
    val rqst00: String,
    val rqst01: Int
)


