# Day 18 — AI Customer Support Triage System

## Project Overview

This project is an AI-powered customer support triage system built with n8n.

The workflow receives a customer support message through a webhook, analyzes it using an AI model, generates validated structured JSON, routes the ticket based on its category, stores the ticket in Google Sheets, sends an automatic acknowledgment, and sends an urgent alert when the ticket priority is **Urgent**.

## Objective

The objective of this project is to build a production-style AI customer support workflow that can automatically:

* Analyze customer support messages
* Identify the support category
* Determine ticket priority
* Detect customer sentiment
* Assign the appropriate department
* Generate a concise summary
* Generate a suggested response
* Store the ticket information
* Send an acknowledgment email
* Alert the support team for urgent tickets

## Workflow

```text
Customer Message
       ↓
    Webhook
       ↓
   AI Analysis
       ↓
   JSON Parsing
       ↓
Switch by Category
       ↓
  Store Ticket
       ↓
Send Acknowledgment
       ↓
Priority = Urgent?
       ↓
  Urgent Alert
```

## Technologies Used

* n8n
* AI/LLM
* Webhook
* Structured JSON
* Switch node
* IF node
* Google Sheets
* Email
* JSON parsing

## Input Format

The webhook accepts customer information in JSON format.

```json
{
  "customer_name": "Ali Khan",
  "email": "ali.khan.test@example.com",
  "message": "My payment was deducted but my order is still showing as unpaid."
}
```

## AI Output

The AI analyzes the customer message and returns structured information containing:

* category
* priority
* sentiment
* department
* summary
* suggested_response

Example:

```json
{
  "category": "Billing",
  "priority": "High",
  "sentiment": "Negative",
  "department": "Billing",
  "summary": "Customer was charged but the order still shows as unpaid.",
  "suggested_response": "We will verify the payment and help resolve the issue."
}
```

## Categories

The workflow supports the following categories:

* Billing
* Technical
* Account
* Shipping
* Product
* Complaint
* General

## Priority Levels

The AI assigns one of the following priority levels:

* Low
* Medium
* High
* Urgent

Urgent tickets are automatically sent to the urgent alert branch.

## Sentiment

The workflow identifies the customer's sentiment as:

* Positive
* Neutral
* Negative
* Angry

## Departments

Tickets are assigned to an appropriate department:

* Billing
* Technical Support
* Customer Service
* Shipping
* Sales

## AI Prompt

The AI was instructed to analyze the customer support message and return only the required structured JSON fields.

The complete prompt used in the workflow is available in:

`prompt.md`

## Structured JSON Schema

The structured output schema is available in:

`schema.json`

The schema ensures that the AI output contains the required fields:

```text
category
priority
sentiment
department
summary
suggested_response
```

## Category Routing

After AI analysis, the ticket is routed using an n8n Switch node based on the `category` field.

The workflow supports:

```text
Billing
Technical
Account
Shipping
Product
Complaint
General
```

## Ticket Storage

After classification, the ticket is stored in Google Sheets.

The stored information includes:

* Customer name
* Email
* Original message
* Category
* Priority
* Sentiment
* Department
* Summary
* Suggested response
* Timestamp

## Customer Acknowledgment

After storing the ticket, the workflow automatically sends an acknowledgment email to the customer.

The email informs the customer that their support request has been received and classified.

## Urgent Alert

An IF node checks whether:

```text
priority = Urgent
```

If the condition is true, an urgent alert is sent to the support team.

The alert contains:

* Customer name
* Customer email
* Category
* Priority
* Sentiment
* Department
* Issue summary

This allows urgent customer issues to receive immediate attention.

## Testing

The workflow was tested using 10 different customer support messages covering multiple categories and priority levels.

The complete test results are available in:

`test-results.md`

The tests covered:

1. Billing issue
2. Technical issue
3. Account/password issue
4. Shipping delay
5. Product information request
6. Urgent complaint
7. Positive/general message
8. Account information update
9. Damaged product
10. Service plan inquiry

## Test Result Verification

Each test was checked for:

* Category
* Priority
* Sentiment
* Department
* Summary
* Suggested response
* Ticket storage
* Email acknowledgment
* Urgent alert when applicable

## Files

```text
Day-18/
│
├── workflow.json
├── prompt.md
├── schema.json
├── test-results.md
└── README.md
```

## How to Run

1. Import `workflow.json` into n8n.
2. Configure the required AI credentials.
3. Configure Google Sheets credentials.
4. Configure the email credentials.
5. Activate or test the webhook.
6. Send a customer support message in JSON format.
7. Execute the workflow.
8. Verify the AI classification.
9. Verify that the ticket is stored in Google Sheets.
10. Verify the acknowledgment email.
11. Test an Urgent message to verify the alert.

## Conclusion

The Day 18 AI Customer Support Triage System demonstrates how AI can be integrated into an automation workflow to classify, route, store, and respond to customer support requests.

The workflow uses structured AI output and automated routing to make customer support processing more consistent and efficient.
