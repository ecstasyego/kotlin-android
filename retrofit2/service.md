

## Examples
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import android.widget.Toast
import androidx.activity.ComponentActivity
import okhttp3.ResponseBody
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.InputStreamReader

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val intent = Intent(this, RemoteService::class.java)
        intent.putExtra("fileName", "005930.csv")  // 파일명 전달
        startService(intent)
    }
}

class RemoteService : Service() {

    private val retrofit = Retrofit.Builder()
        .baseUrl("https://raw.githubusercontent.com/")
        .addConverterFactory(ScalarsConverterFactory.create())
        .build()

    private val apiService = retrofit.create(GithubApiService::class.java)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val fileName = intent?.getStringExtra("fileName") ?: "default.csv"

        val call = apiService.downloadCsvFile(fileName)
        call.enqueue(object : Callback<ResponseBody> {
            override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
                if (response.isSuccessful) {
                    val body = response.body()
                    body?.let {
                        val reader = BufferedReader(InputStreamReader(it.byteStream()))
                        val csvData = mutableListOf<List<String>>()
                        reader.useLines { lines ->
                            lines.forEach { line ->
                                val values = line.split(",").map { it.trim() }
                                csvData.add(values)
                            }
                        }

                        Toast.makeText(applicationContext, "CSV Downloaded Successfully", Toast.LENGTH_SHORT).show()
                    }
                } else {
                    Toast.makeText(applicationContext, "Download Failed: ${response.message()}", Toast.LENGTH_SHORT).show()
                }
            }

            override fun onFailure(call: Call<ResponseBody>, t: Throwable) {
                Toast.makeText(applicationContext, "Download Error: ${t.message}", Toast.LENGTH_SHORT).show()
            }
        })

        return START_NOT_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
}


interface GithubApiService {
    @GET("ecstasyego/CSV/main/{fileName}")
    fun downloadCsvFile(@Path("fileName") fileName: String): Call<ResponseBody>
}
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
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

        <service android:name=".RemoteService" android:enabled="true" android:exported="false"/>

    </application>
</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</LinearLayout>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("com.squareup.okhttp3:okhttp:4.9.3")
}
```



<br>



### Example02: BroadcastReceiver
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

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import android.widget.Toast
import androidx.activity.ComponentActivity
import okhttp3.ResponseBody
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.InputStreamReader

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val intent = Intent(this, RemoteService::class.java)
        intent.putExtra("fileName", "005930.csv")  // 파일명 전달
        startService(intent)
    }
}

class RemoteService : Service() {

    private val retrofit = Retrofit.Builder()
        .baseUrl("https://raw.githubusercontent.com/")
        .addConverterFactory(ScalarsConverterFactory.create())
        .build()

    private val apiService = retrofit.create(GithubApiService::class.java)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val fileName = intent?.getStringExtra("fileName") ?: "default.csv"

        val call = apiService.downloadCsvFile(fileName)
        call.enqueue(object : Callback<ResponseBody> {
            override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
                if (response.isSuccessful) {
                    val body = response.body()
                    body?.let {
                        val reader = BufferedReader(InputStreamReader(it.byteStream()))
                        val csvData = mutableListOf<List<String>>()
                        reader.useLines { lines ->
                            lines.forEach { line ->
                                val values = line.split(",").map { it.trim() }
                                csvData.add(values)
                            }
                        }

                        Toast.makeText(applicationContext, "CSV Downloaded Successfully", Toast.LENGTH_SHORT).show()
                    }
                } else {
                    Toast.makeText(applicationContext, "Download Failed: ${response.message()}", Toast.LENGTH_SHORT).show()
                }
            }

            override fun onFailure(call: Call<ResponseBody>, t: Throwable) {
                Toast.makeText(applicationContext, "Download Error: ${t.message}", Toast.LENGTH_SHORT).show()
            }
        })

        return START_NOT_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
}


interface GithubApiService {
    @GET("ecstasyego/CSV/main/{fileName}")
    fun downloadCsvFile(@Path("fileName") fileName: String): Call<ResponseBody>
}
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
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

        <service android:name=".RemoteService" android:enabled="true" android:exported="false"/>

    </application>
</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</LinearLayout>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("com.squareup.okhttp3:okhttp:4.9.3")
}
```



<br>



### Example03: BroadcastReceiver + RecyclerView
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

import android.annotation.SuppressLint
import android.app.Service
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Bundle
import android.os.IBinder
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import okhttp3.ResponseBody
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.scalars.ScalarsConverterFactory
import retrofit2.http.GET
import retrofit2.http.Path
import java.io.BufferedReader
import java.io.File
import java.io.FileReader
import java.io.InputStreamReader

class MainActivity : ComponentActivity() {
    private lateinit var recyclerView: RecyclerView
    private lateinit var adapter: CsvAdapter
    private val csvData = mutableListOf<List<String>>()

    private val csvDownloadReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val filePath = intent?.getStringExtra("filePath")
            filePath?.let {
                // Read CSV data from the file
                val file = File(it)
                if (file.exists()) {
                    val reader = BufferedReader(FileReader(file))
                    val data = mutableListOf<List<String>>()
                    reader.useLines { lines ->
                        lines.forEach { line ->
                            val values = line.split(",").map { it.trim() }
                            data.add(values)
                        }
                    }
                    csvData.clear()
                    csvData.addAll(data)
                    adapter.notifyDataSetChanged()
                    Toast.makeText(applicationContext, "CSV Data Loaded", Toast.LENGTH_SHORT).show()
                }
            }
        }
    }

    @SuppressLint("NewApi")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // PRESENTATION
        adapter = CsvAdapter(csvData)
        recyclerView = findViewById(R.id.recyclerView)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = adapter

        // SERVICE
        val intent = Intent(this, RemoteService::class.java)
        intent.putExtra("fileName", "005930.csv")
        startService(intent)

        val filter = IntentFilter("CSV_DOWNLOAD_COMPLETED")
        registerReceiver(csvDownloadReceiver, filter, RECEIVER_NOT_EXPORTED)
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(csvDownloadReceiver)
    }
}

class CsvAdapter(private val items: List<List<String>>) : RecyclerView.Adapter<CsvAdapter.CsvViewHolder>() {

    inner class CsvViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
        val textView = itemView.findViewById<TextView>(android.R.id.text1)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CsvViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(android.R.layout.simple_list_item_1, parent, false)
        return CsvViewHolder(view)
    }

    override fun onBindViewHolder(holder: CsvViewHolder, position: Int) {
        holder.textView.text = items[position].joinToString(", ")
    }

    override fun getItemCount(): Int {
        return items.size
    }
}

class RemoteService : Service() {
    private val retrofit = Retrofit.Builder()
        .baseUrl("https://raw.githubusercontent.com/")
        .addConverterFactory(ScalarsConverterFactory.create())
        .build()

    private val apiService = retrofit.create(GithubApiService::class.java)

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        val fileName = intent?.getStringExtra("fileName") ?: "default.csv"

        val call = apiService.downloadCsvFile(fileName)
        call.enqueue(object : Callback<ResponseBody> {
            override fun onResponse(call: Call<ResponseBody>, response: Response<ResponseBody>) {
                if (response.isSuccessful) {
                    val body = response.body()
                    body?.let {
                        val csvData = mutableListOf<List<String>>()
                        val file = File(applicationContext.filesDir, fileName)

                        // Write the CSV data to a file
                        file.bufferedWriter().use { writer ->
                            val reader = BufferedReader(InputStreamReader(it.byteStream()))
                            reader.useLines { lines ->
                                lines.forEach { line ->
                                    val values = line.split(",").map { it.trim() }
                                    csvData.add(values)
                                    writer.write(line)
                                    writer.newLine()
                                }
                            }
                        }

                        // Send the file path through the Intent
                        val broadcastIntent = Intent("CSV_DOWNLOAD_COMPLETED").setPackage(packageName)
                        broadcastIntent.putExtra("filePath", file.absolutePath)
                        sendBroadcast(broadcastIntent)

                        Toast.makeText(applicationContext, "CSV Downloaded Successfully", Toast.LENGTH_SHORT).show()
                    }
                } else {
                    Toast.makeText(applicationContext, "Download Failed: ${response.message()}", Toast.LENGTH_SHORT).show()
                }
            }

            override fun onFailure(call: Call<ResponseBody>, t: Throwable) {
                Toast.makeText(applicationContext, "Download Error: ${t.message}", Toast.LENGTH_SHORT).show()
            }
        })

        return START_NOT_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
}



interface GithubApiService {
    @GET("ecstasyego/CSV/main/{fileName}")
    fun downloadCsvFile(@Path("fileName") fileName: String): Call<ResponseBody>
}
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" />

    <application
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

        <service android:name=".RemoteService" android:enabled="true" android:exported="false"/>

    </application>
</manifest>
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.retrofit2:converter-scalars:2.9.0")
    implementation("com.squareup.okhttp3:okhttp:4.9.3")
}
```

