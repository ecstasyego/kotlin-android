```kotlin
import java.text.SimpleDateFormat
import java.time.format.DateTimeFormatter
import java.util.Calendar
import java.util.Date
import java.util.Locale
import java.time.LocalDateTime

// SimpleDateFormat
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("2024-12-18 15:30:00")
SimpleDateFormat("EEE MMM dd HH:mm:ss z yyyy", Locale.ENGLISH).parse("Wed Dec 18 16:03:44 KST 2024")
SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(Date())

// LocalDateTime
LocalDateTime.now()
LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"))


// Date Range
val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val dateRange = startDate..endDate

dateFormat.parse("2024-12-15") in startDate..endDate // time check

val calendar = Calendar.getInstance().apply{ time = startDate }
while (calendar.time <= endDate) {
    println(dateFormat.format(calendar.time))  // date in date-ragnge
    calendar.add(Calendar.DATE, 1)  // Move to the next day
}
```
