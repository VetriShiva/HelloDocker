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