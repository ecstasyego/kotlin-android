### String

```kotlin
String.format(Locale.getDefault(), "%,.2f", 100000.123456)
```


### Date
```kotlin
import java.text.SimpleDateFormat
import java.util.Date

// Date Object
Date(1701388800000L) // from timestamp
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2024-12-01 00:00:00") // from timestring

// Timestamp: ~ milliseconds after 1970-01-01 00:00:00 UTC
Date(1701388800000L).time
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2024-12-01 00:00:00").time

// Timestring
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(Date(1701388800000L))
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2024-12-01 00:00:00"))
```
