###UTC and UTC-SLS standards
Coordinated Universal Time (UTC) is the world's principal time standard for regulating clocks and time. It has effectively replaced the Greenwich Mean Time (GMT), which is the yearly average of the time at the Royal Observatory Greenwich.

UTC uses the International Atomic Time (IAT) in order to keep the correct time, which is the mean time of 400 atomic clocks around the globe.

Although we can measure time with great accuracy, the Earth rotating speed isn't that stable. Due to earthquakes, tides, etc., the time we measure isn't synchronized with the astronomical time – that is, what the time should be according to the Earth's position relative to the Sun. This time is corrected by the insertion of leap seconds on an irregular basis. The Coordinated Universal Time with Smoothed Leap Seconds (UTC-SLS) standard is effectively the same as the UTC standard, the difference being that it takes into account the leap seconds correction.

Time zones
UTC provides the correct time for the longitude equal to zero degrees, but the rest of the world has its own local time. Thus, the countries are divided into different Time Zones, whose local time ranges from UTC minus 12 hours to UTC plus 14 hours. The local time of a time zone is represented as UTC+<hours:minutes> or UTC-<hours:minutes>. For example:


UTC-05:00

UTC+00:00

UTC+02:00

UTC+05:45

Times zones may also be denoted by alphabetic abbreviations, for example "MUT" stands for "Mauritius Time", that is UTC+04:00.

###The ISO 8601 date and time representation

A date is formatted as YYYY-MM-DD (extended) or YYYYMMDD (basic), where Y denotes the year digits, M denotes the month digits, and D denotes the day digits. For example:

`2000-01-01

2001-12-31

20100207

20121120`


ime is formatted as Thh:mm:ss.fff, Thh:mm:ss (extended), Thhmmss or Thhmmss.fff (basic), where h denotes the hour digits, m the minutes digits, s the seconds, and f the seconds' decimal digits (depending on the accuracy). T is used as an indicator that the string that follows represents time. Note that T can be omitted for the extended notation. Also, note that midnight may only be referred as 00:00:00 and not 24:00:00. For example:

```kotlin
/*
T12:00:15
T120015
T14:01:34.55
14:01:34.55
T170517.3432
T00:00:00
00:00:00
*/

```

The time zone indicator is formatted as Z (for UTC standard time), ±hh:mm, ±hhmm, or ±hh. For example:

`Z

+07:00

+0700

-02:00

-02`

All time data have a fixed number of digits, and if the actual numbers have less digits, then they have to be padded with leading zeros. For example, the correct date format is 2022-02-02 and not 2022-2-2.

Date and time together are represented in the format of <date>T<time><time zone indicator>. The time indicator T cannot be omitted here, while the time zone indicator is optional.

Following are some examples of the ISO 8601 date-time representation:

```kotlin
/*
2022-02-16T16:31:03+02:00
2022-02-16T16:31:03Z
20220216T163103Z
*/

```

### The ISO 8601 duration representation
Time duration is represented as **P<n>Y<n>M<n>DT<n>H<n>M<n>S**, where P denotes that this string represents time duration, Y denotes years, M months, D days, T is the days-time divider, H denotes hours, the M after T denotes minutes, and S – seconds. <n> denotes the value of the time element that follows it. For example:


`P2Y11M5DT4H10M3S`

The string denotes the duration of 2 years, 11 months, 5 days, 4 hours, 10 minutes, and 3 seconds.

If the value of a time item is zero, it can be omitted. For example:

```kotlin
P4Y      Duration of 4 years
PT3H5M   Duration of 3 hours and 5 minutes
P7DT6H   Duration of 7 days and 6 hours
```
The last item may also have decimal digits. For example:

```kotlin
P12.5Y         Duration of 12.5 years
PT2H3M10.23S   Duration of 2 hours, 3 minutes and 10.23 seconds
P2M7.3D        Duration of 2 months and 7.3 days

```

### The ISO 8601 time interval representation
Time intervals can be expressed in the following formats:

`1) <Starting date-time>/<Ending date-time>`

For example:

```kotlin
/*
2022-12-01T00:00:00Z/2022-12-31T23:59:59Z
2000-01-01T00:00:00+03:00/2011-01-01T00:00:00+03:00
2022-02-01T14:00:00-05:00/2022-02-01T14:15:59-05:00
*/

```
`2) <Starting date-time>/<Duration>`

For example:

```kotlin
/*
2000-01-01T00:00:00Z/P3Y10M4DT2H11M13S
2012-07-19T17:12:00Z/PT35M
*/

```
`3) <Duration>/<Ending date-time>`

For example:

```kotlin
/*
P1Y1M2DT21H9M7S/2000-01-01T00:00:00Z
P3Y/2021-01-01T01:00:00Z
*/

```


### How to use the library in a project
In your Gradle project, add the Maven Central repository with:

```kotlin
repositories {
mavenCentral()
}
```
Then add to the dependencies block (for example, for version 0.3.2 here):

```kotlin
dependencies {
implementation("org.jetbrains.kotlinx:kotlinx-datetime:0.3.2")
}
```
In your Maven project, add the dependency:

```kotlin
<dependency>
    <groupId>org.jetbrains.kotlinx</groupId>
    <artifactId>kotlinx-datetime-jvm</artifactId>
    <version>0.3.2</version>
</dependency>
```
The latest kotlinx-datetime version can be found in the Maven Central depository.

In all source files, add the following import line:


**import kotlinx.datetime.***

### Instant and Clock classes
The Instant class is used to represent a specific moment in time with greater accuracy; it complies with the UTC-SLS and ISO 8601 standards.

An Instant of a specific moment can be created from an ISO 8601 string either with the use of the Instant.parse() function

```kotlin
val instant1 = Instant.parse("2022-02-01T22:10:01.324Z")
println(instant1)  // 2022-02-01T22:10:01.324Z
```
or use of the String extension function toInstant() as in the following examples:

```kotlin
val instant2 = "2022-02-01T22:10:01.324Z".toInstant()
println(instant2)  // 2022-02-01T22:10:01.324Z

val instant3 = "2022-02-01T09:10:01.324+10:00".toInstant()
println(instant3)  // 2022-01-31T23:10:01.324Z
```
As you can see from the last example (instant3), a non-UTC date-time input is correctly converted to UTC.

In case the provided string isn't a valid ISO 8601 string, a Kotlinx datetime DateTimeFormatException exception is thrown caused by a java.time.format DateTimeParseException exception. The same occurs even if the provided string is valid but the represented date isn't a real date, as in the following example:

```kotlin
// The following line throws a DateTimeParseException
// because Feb 29, 2001 isn't a real date
val instant = Instant.parse("2001-02-29T00:00:00Z")
```
Note that due to the current Kotlinx datetime library design this exception can't be directly caught. Instead the RuntimeException should be caught.

In order to create an Instant of the current date and time, we can use the Clock class as a time provider:

`val currentInstant = Clock.System.now()`

### TimeZone classes
The TimeZone class is used to keep time zone information. It can be initialized in many ways.

The **currentSystemDefault()** member function provides the computer system time zone, while UTC sets the time zone to UTC (denoted as Z in ISO 8601), as in the following example:

```kotlin
val tz1 = TimeZone.currentSystemDefault()  // The computer system time zone

val tz2 = TimeZone.UTC                     // UTC time zone
println(tz2)                               // Z
```
Any other time zone is set with the help of the of() member function, which takes a string as a parameter. It can be either the UTC offset (e.g., "UTC-3" or "UTC-03:00") or the time zone name (e.g., "Europe/Rome"). You can find the valid time zone names in the tz database. For example:
```kotlin
val tz3 = TimeZone.of("Europe/Paris")      // Paris, France time zone
println(tz3)                               // Europe/Paris

val tz4 = TimeZone.of("UTC+2")             // UTC + 2 hours time zone
println(tz4)                               // UTC+02:00

```
In case the provided parameter of the of() function isn't valid, the IllegalTimeZoneException is thrown.

### Basic instant operations
In order to get the time difference between 2 instants, we can use the until(other: Instant, unit: DateTimeUnit, timezone: TimeZone) member function, where instant is an instant to compare with, unit of DateTimeUnit class is explained below, and timezone is a time zone.

This function returns the difference as a whole number in the specified time or date units. The returned value is:

+ positive or zero if instant1 occurs earlier than instant2,

+ negative or zero if instant1 occurs later than instant2,

+ zero if instant1 is equal to instant2.

The DateTimeUnit class can be used to specify the time or date unit: CENTURY, YEAR, QUARTER, MONTH, WEEK, DAY, HOUR, MINUTE, SECOND, MILLISECOND, MICROSECOND, and NANOSECOND. The time zone parameter of the until() function can be omitted if the DateTimeUnit is smaller than DAY.

For example, check out the following code:

```kotlin
val instant1 = Instant.parse("2022-01-01T00:00:00Z")
val instant2 = Instant.parse("2022-02-03T04:05:06Z")

// instant2 - instant1
println(instant1.until(instant2, DateTimeUnit.WEEK, TimeZone.UTC))    // 4
println(instant1.until(instant2, DateTimeUnit.DAY, TimeZone.UTC))     // 33
println(instant1.until(instant2, DateTimeUnit.HOUR, TimeZone.UTC))    // 796
println(instant1.until(instant2, DateTimeUnit.HOUR))                  // 796
println(instant1.until(instant2, DateTimeUnit.SECOND, TimeZone.UTC))  // 2865906
println(instant1.until(instant2, DateTimeUnit.SECOND))                // 2865906

// instant1 - instant2
println(instant2.until(instant1, DateTimeUnit.WEEK, TimeZone.UTC))    // -4
```
Subtracting two Instant objects creates a Kotlin Duration class object. The latter class of the kotlin.time package is used for keeping the time difference between two instants. For example:

```kotlin
val instant1 = Instant.parse("2000-01-01T20:00:00Z")
val instant2 = Instant.parse("2012-10-14T05:20:30Z")

val duration: Duration = instant2 - instant1
println(duration)
// 8124d 16h 29m 37.716s
```
Two different Instant objects can be directly compared by using common logic operators, as in the following example:

```kotlin
val instant1 = Instant.parse("2022-01-01T00:00:00Z")
val instant2 = Instant.parse("2022-02-03T04:05:06Z")

if (instant1 > instant2)
println(instant1)
else
println(instant2)

// Prints 2022-02-03T04:05:06Z
```
### DateTimePeriod class
The DateTimePeriod class is used for keeping the difference between two Instant objects split into date and time components. These can be accessed from the relevant properties named years, months, days, hours, minutes, seconds, and nanoseconds. Printing a DateTimePeriod object gives the difference as an ISO 8601 duration representation.

A difference between two Instant objects can be acquired with the use of the periodUntil(other: Instant, timeZone: TimeZone) member function, where other is another Instant and timezone is a time zone. For example:

```kotlin
val instant1 = Instant.parse("2000-01-01T20:00:00Z")
val instant2 = Instant.parse("2000-10-14T00:00:00Z")

val period: DateTimePeriod = instant1.periodUntil(instant2, TimeZone.UTC)

println(period)
// P9M12DT4H

println("Months: ${period.months} Days: ${period.days} Hours: ${period.hours}")
// Months: 9 Days: 12 Hours: 4
```
An important use of the DateTimePeriod class is to apply a time offset to an Instant with the Instant.plus() function, which adds an amount of time to an Instant, and the Instant.minus() function, which subtracts an amount of time from an Instant.

```kotlin
val instant = Instant.parse("2000-01-01T00:00:00Z")
println(instant)
// 2000-01-01T00:00:00Z

val period: DateTimePeriod = DateTimePeriod(years = 1, months = 1, days = 1, hours = 1, minutes = 1, seconds = 1, nanoseconds = 123456789L)
println(period)
// P1Y1M1DT1H1M1.123456789S

val after = instant.plus(period, TimeZone.UTC)

println(after)
// 2001-02-02T01:01:01.123456789Z

val before = instant.minus(period, TimeZone.UTC)

println(before)
// 1998-11-29T22:58:58.876543211Z
```
Following are some examples to depict the differences between the Duration (kotlin.time package) and the DateTimePeriod (kotlinx) classes:

```kotlin
val instant1 = Instant.parse("2100-01-01T00:00:00Z")
val instant2 = Instant.parse("2105-07-09T15:23:40Z")

val duration: Duration = instant2 - instant1
println(duration)                                             // 2015d 15h 23m 40s
println(duration.inWholeDays)                                 // 2015
println(duration.inWholeHours)                                // 48375

println( instant1.periodUntil(instant2, TimeZone.UTC) )       // P5Y6M8DT15H23M40S
println( instant1.periodUntil(instant2, TimeZone.UTC).days )  // 8
```