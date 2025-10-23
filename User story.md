Scheduled Payments API Scenarios and Acceptance Criteria

User Story

As a Weyay loan customer,
I want to set, update, or delete my scheduled payment deduction date for my active loan,
So that my EMI deduction happens on a date that suits my salary cycle within the allowed range (20th–31st).


---

API Behavior Summary

Endpoint: PUT /loans/v1/details/{loanNumber}/scheduled-payment

Request Body:

{
  "scheduledDay": 25
}

Accepts nullable int from 20 to 31.

If scheduledDay = null, removes the scheduled day (reverts to default behavior).

The system automatically computes the nextDueDate based on the given day and the current or future month.



---

Acceptance Criteria

Scenario 1: Set a new scheduled payment day

Given a user has an active non-salaried loan
When the user calls PUT /loans/v1/details/{loanNumber}/scheduled-payment with

{ "scheduledDay": 25 }

Then

Validate loan is active and not overdue.

Validate 25 is within [20–31].

Compute the next valid nextDueDate.

Return:

{
  "loanNumber": "1234567890",
  "scheduledDay": 25,
  "nextDueDate": "2025-02-25",
  "status": "UPDATED"
}

Audit the change with timestamp, user, and previous value.



---

Scenario 2: Update an existing scheduled payment day

Given user already has a scheduled day (e.g., 25)
When the user calls with

{ "scheduledDay": 28 }

Then

Validate 28 is within [20–31].

Update stored value and recompute nextDueDate.

Return:

{
  "loanNumber": "1234567890",
  "scheduledDay": 28,
  "nextDueDate": "2025-02-28",
  "status": "UPDATED"
}

Append audit record with old vs new values.



---

Scenario 3: Remove scheduled payment day

Given user had a scheduled day configured
When the user calls with

{ "scheduledDay": null }

Then

Remove the custom schedule.

Revert to default EMI deduction day.

Return:

{
  "loanNumber": "1234567890",
  "scheduledDay": null,
  "status": "REMOVED"
}

Mark in audit as Deleted by customer.



---

Scenario 4: Invalid day value

When user provides an invalid day (e.g., 15)
Then

Return HTTP 400 Bad Request:

{
  "error": "INVALID_DAY",
  "message": "Scheduled day must be between 20 and 31."
}



---

Scenario 5: Loan not eligible

Given the loan is closed, delinquent, or under restructuring
When the user calls the API
Then

Return HTTP 403 Forbidden:

{
  "error": "LOAN_INELIGIBLE",
  "message": "Scheduled payment update not allowed for this loan."
}



---

Scenario 6: Edge dates (February & 31st)

When user sets scheduledDay = 31 for February
Then

Adjust `nextDue