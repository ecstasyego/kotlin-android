## Time-Checker
`time-checker`
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val dateRange = startDate..endDate

dateFormat.parse("2024-12-15") in startDate..endDate // time check
```

`time-formatter:forloop`
```kotlin
import java.util.Calendar
import java.util.Date
import java.text.SimpleDateFormat

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-10")

val calendar = Calendar.getInstance().apply { time = startDate }
for (i in 0 until 100) {
    println(SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time)) 
    calendar.add(Calendar.DATE, -1)
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
    println(SimpleDateFormat("EEE yyyy-MM-dd").format(date)) 
}
```

`time-formatter:whileloop`
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")

val calendar = Calendar.getInstance().apply{ time = startDate }
while (calendar.time <= endDate) {
    println(SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time))
    calendar.add(Calendar.DATE, 1)
}
```

<br>


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

`daterange:forloop`
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

`daterange:whileloop`
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

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
val endDate = dateFormat.parse("2024-12-31")

val calendar = Calendar.getInstance().apply{ time = startDate }
while (calendar.time <= endDate) {
    // println(SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time))
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
    
    calendar.add(Calendar.DATE, 1)
}
```

<br>

## Date ~ size

### startDate ~ size: forward

```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val today = Date()


val numdays = 100 
val dateList = mutableListOf<Date>()
val calendar = Calendar.getInstance().apply { time = startDate }
for (i in 0 until numdays) {
    dateList.add(calendar.time) // SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time)
    calendar.add(Calendar.DATE, 1)
}
```

### size ~ endDate: backward
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val today = Date()


val numdays = 100 
val dateList = mutableListOf<Date>()
val calendar = Calendar.getInstance().apply { time = endDate }
for (i in 0 until numdays) {
    dateList.add(calendar.time) // SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time)
    calendar.add(Calendar.DATE, -1)
}
```

### today ~ size: forward
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val today = Date()


val numdays = 100 
val dateList = mutableListOf<Date>()
val calendar = Calendar.getInstance().apply { time = today }
for (i in 0 until numdays) {
    dateList.add(calendar.time) // SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time)
    calendar.add(Calendar.DATE, 1)
}
```


### size ~ today: backward
```kotlin
import java.text.SimpleDateFormat
import java.util.Calendar
import java.util.Date

val dateFormat = SimpleDateFormat("yyyy-MM-dd")
val startDate = dateFormat.parse("2024-12-01")
val endDate = dateFormat.parse("2024-12-31")
val today = Date()


val numdays = 100 
val dateList = mutableListOf<Date>()
val calendar = Calendar.getInstance().apply { time = today }
for (i in 0 until numdays) {
    dateList.add(calendar.time) // SimpleDateFormat("EEE yyyy-MM-dd").format(calendar.time)
    calendar.add(Calendar.DATE, -1)
}
```


