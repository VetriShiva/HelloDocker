

Hey Naji, noticed the 403 counts are still on the higher side — maybe some repeated MAS calls (like firm2D reminders). Let’s review when you get a chance 😊
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


private val KUWAIT_ZONE_ID = ZoneId.of("Asia/Kuwait")

/**
 * Safely computes a valid date within the given month.
 * Example: if day=31 but the month has 30 days, adjusts to 30.
 */
fun getValidDate(year: Int, month: Int, day: Int): ZonedDateTime {
    val safeMonth = month.coerceIn(1, 12)
    val yearMonth = YearMonth.of(year, safeMonth)
    val safeDay = day.coerceIn(1, yearMonth.lengthOfMonth())

    return yearMonth
        .atDay(safeDay)
        .atStartOfDay(KUWAIT_ZONE_ID)
}

/**
 * Computes the next scheduled payment date for a loan, respecting autopay day
 * and month rollovers.
 */
fun LoanData.toScheduledPayment(): LoanScheduledPayment? {
    val repayDay = autoRepayDay?.toIntOrNull() ?: return null
    if (repayDay <= 0) return null

    val baseDate = upcomingInstallmentDate ?: return null
    val nextDate = getValidDate(
        year = baseDate.year,
        month = baseDate.monthValue + 1, // move to next month
        day = repayDay
    )

    return LoanScheduledPayment(
        scheduledDay = repayDay,
        scheduledDate = nextDate
    )
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


# Loan EMI Schedule – Next Date Calculation Matrix

> **Rule:**  
> If the selected schedule day is already past in the current month,  
> → Show **due date (1st of next month)**  
> → From next month onward, follow user-selected schedule day.  
> Includes 1-month prepayment & 3-month grace period logic.

| # | Login Date | Schedule Day | Grace (months) | Prepaid (1 mo) | **Next Schedule Date** | **Month+1** | **Month+2** | Reason |
|---|-------------|---------------|----------------|----------------|-------------------------|--------------|--------------|--------|
| 1 | 2025-11-19 | 20 | 0 | ❌ | 2025-11-20 | 2025-12-20 | 2026-01-20 | In future same month |
| 2 | 2025-11-21 | 20 | 0 | ❌ | **2025-12-01 (Due)** | 2025-12-20 | 2026-01-20 | Past-date → due first |
| 3 | 2025-11-30 | 31 | 0 | ❌ | **2025-12-01 (Due)** | 2025-12-31 | 2026-01-31 | Month end fallback |
| 4 | 2025-12-02 | 31 | 0 | ❌ | 2025-12-31 | 2026-01-31 | 2026-02-28 | 31 → valid month |
| 5 | 2025-12-30 | 25 | 0 | ❌ | **2026-01-01 (Due)** | 2026-01-25 | 2026-02-25 | Past-date → due first |
| 6 | 2025-12-30 | 25 | 0 | ✅ | **2026-01-25** | 2026-02-25 | 2026-03-25 | Prepaid skips due |
| 7 | 2025-12-10 | 31 | 3 | ❌ | **2026-03-31** | 2026-04-30 | 2026-05-31 | 3-month grace applied |
| 8 | 2026-01-30 | 31 | 0 | ❌ | 2026-01-31 | 2026-02-28 | 2026-03-31 | Valid same month |
| 9 | 2026-01-30 | 31 | 0 | ✅ | **2026-02-28** | 2026-03-31 | 2026-04-30 | Prepaid skips Jan |
| 10 | 2026-02-27 | 31 | 0 | ❌ | 2026-02-28 | 2026-03-31 | 2026-04-30 | Feb fallback |
| 11 | 2026-02-28 | 20 | 0 | ❌ | **2026-03-01 (Due)** | 2026-03-20 | 2026-04-20 | Past-date → due |
| 12 | 2026-02-01 | 20 | 0 | ❌ | 2026-02-20 | 2026-03-20 | 2026-04-20 | Future same month |

---

✅ **Key behaviors:**
- Past schedule → use **due date (1st next month)**.
- Grace → defer full months (resume after grace).
- Prepaid → skip immediate next schedule.
- 31st day → fallback to last day of month (e.g., Feb 28).