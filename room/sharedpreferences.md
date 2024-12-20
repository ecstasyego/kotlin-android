```kotlin
import android.content.Context
import android.content.SharedPreferences

// 파일 열기 및 저장
val sharedPreferences = getSharedPreferences("my_preferences", Context.MODE_PRIVATE) // /data/data/<package-name>/shared_prefs/<preferences-name>.xml
val editor = sharedPreferences.edit()
editor.putString("username", "JohnDoe")
editor.putBoolean("is_logged_in", true)
editor.putInt("user_age", 30)
editor.apply()  // 데이터 저장

// SharedPreferences
val sharedPreferences = getSharedPreferences("my_preferences", Context.MODE_PRIVATE)
val username = sharedPreferences.getString("username", "default")
val isLoggedIn = sharedPreferences.getBoolean("is_logged_in", false)
val userAge = sharedPreferences.getInt("user_age", 0)
```
