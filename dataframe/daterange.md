```kotlin
import java.util.Calendar
import java.util.Date
import java.text.SimpleDateFormat

fun Date.nextDay(): Date {
    val calendar = Calendar.getInstance()
    calendar.time = this
    calendar.add(Calendar.DATE, 1)
    return calendar.time
}

val dateFormat = SimpleDateFormat("yyyy-MM-dd")

val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-10")
val dateRange = generateSequence(startDate) { it.nextDay() }.takeWhile { it <= endDate }

for (date in dateRange) {
    println(dateFormat.format(date)) 
}
```
