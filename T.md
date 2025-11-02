

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

```
Here’s a clear and professional task title and description you can use for your Jira, Azure DevOps, or internal tracker:
Task Title:
Enhance Loan Details API to Include Loan Payment Details (Repay Date & Due Date)
Task Description:
As part of the loan module enhancement, the Loan Details API needs to be updated to include loan payment details, specifically the repayment date and due date for each loan account.
Objectives:
Extend the existing Loan Details API response model to include:
repayDate: the actual date when the last or next payment was made/expected.
dueDate: the scheduled due date for the next installment.
Ensure backward compatibility with existing clients.
Populate these fields based on loan repayment schedule data or related transaction history.
Validate and handle null values gracefully for loans without a repayment yet.
Update API documentation (Swagger / Postman / GraphQL schema) accordingly.
Acceptance Criteria:
API response includes both repayDate and dueDate fields.
For active loans, both fields return valid ISO date format (e.g., YYYY-MM-DD).
For newly created loans with no repayment yet, repayDate should be null.
Unit and integration tests updated and passed.
API contract version updated (if applicable).
Would you like me to add a technical implementation note (e.g., where to fetch these dates — from repayment schedule table or loan transaction history)?
```