
## Timestamp
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


### Example
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
SimpleDateFormat("EEE MMM dd HH:mm:ss z yyyy", Locale.getDefault()).parse("Wed Dec 18 16:03:44 KST 2024")
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


### module

```kotlin
import java.text.SimpleDateFormat
import java.time.format.DateTimeFormatter
import java.util.Calendar
import java.util.Date
import java.util.Locale
import java.time.LocalDateTime


class DateRange(start:String? = null, end:String? = null, periods:Int? = null){
    val dateFormat = SimpleDateFormat("yyyy-MM-dd", Locale.getDefault())
    var startDate:Date = if (periods != null && start == null && end != null) dateFormat.parse("0000-01-01") else dateFormat.parse(start?: dateFormat.format(Date()))
    var endDate:Date = if (periods != null && start != null && end == null) dateFormat.parse("9999-12-31") else dateFormat.parse(end?: dateFormat.format(Date()))
    
    val values = mutableListOf<Map<String, String>>()
    init{
        var cnt:Int = 0
        var calendar = Calendar.getInstance().apply{ time = startDate }
        while (calendar.time <= endDate) {
            values.add(
                mapOf(
                    "index" to cnt.toString(), 
                    "date" to dateFormat.format(calendar.time),
                    "year" to calendar.get(Calendar.YEAR).toString(),
                    "month" to calendar.get(Calendar.MONTH).toString(),
                    "day" to calendar.get(Calendar.DAY_OF_MONTH).toString(),
                )
            )

            // iter-condition
            if (periods != null){ if (periods == cnt + 1) break }

            // dayshift
            val dayshift = if (startDate < endDate) 1 else -1 
            calendar.add(Calendar.DATE, dayshift)
            cnt++
        }        
    }
}


val dateRange = DateRange(start="2024-01-10").values
for ( (idx, date) in dateRange.indices.zip(dateRange)){
    date
}
```
