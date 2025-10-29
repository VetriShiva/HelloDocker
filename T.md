```
data class LoanScheduledPayment(
    val autoRepayDay: Int?,
    val nextDate: LocalDate
)

fun LoanData.toScheduledPayment(today: LocalDate = LocalDate.now()): LoanScheduledPayment {
    val autoDay = autoRepayDay
    val baseDate = upcomingInstallmentDate

    val nextDate = when {
        // Case 1: Auto repay not configured
        autoDay == null -> baseDate

        // Case 2: Fixed date in same or next month
        else -> {
            // Determine the target month
            val thisMonthTarget = getSafeDate(today.year, today.monthValue, autoDay)
            val nextMonthTarget = getSafeDate(today.year, today.monthValue + 1, autoDay)

            // If today is before or equal to auto repay day, use current month
            if (today.isBefore(thisMonthTarget) || today.isEqual(thisMonthTarget))
                thisMonthTarget
            else
                nextMonthTarget
        }
    }

    return LoanScheduledPayment(autoDay, nextDate)
}

/**
 * Returns a valid date even if the requested day (e.g. 30 or 31)
 * doesn't exist in the given month (like February).
 */
fun getSafeDate(year: Int, month: Int, day: Int): LocalDate {
    val ym = YearMonth.of(year, month.coerceIn(1, 12))
    val validDay = minOf(day, ym.lengthOfMonth()) // handles 28/30/31 gracefully
    return ym.atDay(validDay)
}
```

```
### Subject: Request to Update | Synthetic Monitoring 2.5.x

**Dear IT Service Management Team,**

Hope you're doing well. 😊  

Kindly find below the updated JAR for Synthetic Monitoring:  

📦 [performance-monitoring-2.5.13-jar-with-dependencies.jar](\\link\svom\n\Userdata\Common_Share\ETD_Stage\DVP\Release\PROD\performance-monitoring\performance-monitoring-2.5\performance-monitoring-2.5.13-jar-with-dependencies.jar)

Please help update this on the server when convenient.  
Let me know if you need any additional details or support.  

Thanks a lot for your help! 🙏  

**Best regards,**  
Vetri
```