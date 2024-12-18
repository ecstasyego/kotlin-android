## startDate ~ endDate

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
    println(SimpleDateFormat("EEE yyyy-MM-dd").format(date)) 
}
```



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
    //println(SimpleDateFormat("EEE yyyy-MM-dd").format(date)) 
    val calendar = Calendar.getInstance().apply { time = date }
    val month = calendar.get(Calendar.MONTH)
    val year = calendar.get(Calendar.YEAR)
    val dayofmonth = calendar.get(Calendar.DAY_OF_MONTH)
    val dayofyear = calendar.get(Calendar.DAY_OF_YEAR)
    val hour = calendar.get(Calendar.HOUR)
    val hourofday = calendar.get(Calendar.HOUR_OF_DAY)
    val minute = calendar.get(Calendar.MINUTE)
    val second = calendar.get(Calendar.SECOND)
    val millisecond = calendar.get(Calendar.MILLISECOND)
    val weekofyear = calendar.get(Calendar.WEEK_OF_YEAR)
    val weekofmonth = calendar.get(Calendar.WEEK_OF_MONTH)
    val era = calendar.get(Calendar.ERA)
    val ampm = calendar.get(Calendar.AM_PM)
    val zoneoffset = calendar.get(Calendar.ZONE_OFFSET)
    val dstoffset = calendar.get(Calendar.DST_OFFSET)
}
```


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

fun getQuarter(month: Int): String {
    return when (month) {
        in 0..2 -> "Q1"  // January, February, March
        in 3..5 -> "Q2"  // April, May, June
        in 6..8 -> "Q3"  // July, August, September
        else -> "Q4"     // October, November, December
    }
}


val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-10")
val dateRange = generateSequence(startDate) { it.nextDay() }.takeWhile { it <= endDate }

for (date in dateRange) {
    //println(SimpleDateFormat("EEE yyyy-MM-dd").format(date)) 
    val calendar = Calendar.getInstance().apply { time = date }
    val month = calendar.get(Calendar.MONTH)
    val year = calendar.get(Calendar.YEAR)
    val quarter = getQuarter(calendar.get(Calendar.MONTH))
    val dayofmonth = calendar.get(Calendar.DAY_OF_MONTH)
    val dayofyear = calendar.get(Calendar.DAY_OF_YEAR)
    val hour = calendar.get(Calendar.HOUR)
    val hourofday = calendar.get(Calendar.HOUR_OF_DAY)
    val minute = calendar.get(Calendar.MINUTE)
    val second = calendar.get(Calendar.SECOND)
    val millisecond = calendar.get(Calendar.MILLISECOND)
    val weekofyear = calendar.get(Calendar.WEEK_OF_YEAR)
    val weekofmonth = calendar.get(Calendar.WEEK_OF_MONTH)
    val era = calendar.get(Calendar.ERA)
    val ampm = calendar.get(Calendar.AM_PM)
    val zoneoffset = calendar.get(Calendar.ZONE_OFFSET)
    val dstoffset = calendar.get(Calendar.DST_OFFSET)
}
```

## (start|end)Date ~ size
```kotlin
```




