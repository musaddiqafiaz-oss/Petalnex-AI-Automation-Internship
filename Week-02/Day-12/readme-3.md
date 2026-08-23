# Automated Lead Management System

## Day 12 — Week 2 Internship Assignment

### Project Overview

The **Automated Lead Management System** is an n8n-based automation workflow designed to receive, validate, process, score, and route incoming leads automatically.

The system receives lead information through a webhook and assigns each lead a priority based on its budget and requested service. Depending on the priority, the lead is automatically routed to the appropriate destination.

### Objective

The objective of this project is to build a complete automated lead-management pipeline that can:

* Receive leads through a webhook
* Validate required lead information
* Process and normalize lead data
* Calculate a lead score
* Classify leads as High, Medium, or Low Priority
* Send High Priority leads to the sales team
* Store Medium Priority leads in Google Sheets
* Add Low Priority leads to a nurture/follow-up list
* Send an automatic acknowledgment email to the lead

---

## Technologies Used

* **n8n** — Workflow automation
* **Webhook** — Lead intake
* **JavaScript / Code Node** — Validation, processing, and scoring
* **IF Node** — Validation routing
* **Switch Node** — Lead priority routing
* **Google Sheets** — Lead database and nurture list
* **Gmail** — Sales notifications and acknowledgment emails
* **Postman** — API/webhook testing

---

## Lead Input Fields

The webhook accepts the following fields:

| Field     | Description              |
| --------- | ------------------------ |
| `name`    | Lead's full name         |
| `email`   | Lead's email address     |
| `company` | Lead's company name      |
| `service` | Requested service        |
| `budget`  | Estimated project budget |

### Example Request

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "company": "ABC Solutions",
  "service": "AI Automation",
  "budget": 150000
}
```

---

## Workflow Architecture

```text
Webhook
   ↓
Validation
   ↓
IF Validation Check
   ↓
Lead Processing
   ↓
Lead Scoring
   ↓
Switch / Priority Routing
   ├── High Priority → Sales Notification
   │                         ↓
   │                  Acknowledgment Email
   │
   ├── Medium Priority → Google Sheets
   │                         ↓
   │                  Acknowledgment Email
   │
   └── Low Priority → Nurture / Follow-up List
                             ↓
                       Acknowledgment Email
```

---

## 1. Webhook

The workflow starts with an n8n **Webhook** node configured to receive `POST` requests.

The webhook accepts the lead information in JSON format.

Example:

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "company": "ABC Solutions",
  "service": "AI Automation",
  "budget": 150000
}
```

---

## 2. Lead Validation

The validation step checks whether all required fields are present:

* Name
* Email
* Company
* Service
* Budget

If required information is missing, the lead is marked as invalid and is not processed further.

Valid leads continue to the processing stage.

---

## 3. Lead Processing

The processing stage prepares the lead data for scoring.

The workflow:

* Converts the budget into a numeric value
* Adds a lead status
* Adds a processing timestamp

Example:

```text
Lead Status: New
Budget: 150000
Processed At: Automatically generated timestamp
```

---

## 4. Lead Scoring

Each lead receives a score based on the submitted budget and requested service.

### Budget Scoring

| Budget          | Points |
| --------------- | -----: |
| 150,000 or more |     50 |
| 75,000–149,999  |     30 |
| Below 75,000    |     10 |

### Service Scoring

| Service         | Points |
| --------------- | -----: |
| AI Automation   |     30 |
| Web Development |     20 |
| Other Services  |     10 |

### Priority Classification

|       Score | Priority |
| ----------: | -------- |
| 70 or above | High     |
|       40–69 | Medium   |
|    Below 40 | Low      |

---

## 5. Lead Routing

A **Switch** node checks the calculated priority and routes the lead accordingly.

### High Priority

High Priority leads are sent immediately to the sales notification channel.

The notification contains:

* Lead name
* Email
* Company
* Service
* Budget
* Score
* Priority

### Medium Priority

Medium Priority leads are stored in the Google Sheets lead database.

The stored information includes:

* Name
* Email
* Company
* Service
* Budget
* Score
* Priority
* Status
* Created timestamp

### Low Priority

Low Priority leads are added to a separate nurture/follow-up list.

The list can be used for future follow-up campaigns.

---

## 6. Automatic Acknowledgment

After the lead has been processed and routed, the system automatically sends an acknowledgment email to the lead.

The email confirms that the request has been received and informs the lead that the team will review the request.

This removes the need for manually sending confirmation emails.

---

## Testing

The workflow was tested using **Postman** with different lead inputs.

### High Priority Test

```json
{
  "name": "Ali Khan",
  "email": "ali@example.com",
  "company": "ABC Solutions",
  "service": "AI Automation",
  "budget": 150000
}
```

Expected result:

```text
Score: 80
Priority: High
→ Sales Notification
→ Acknowledgment Email
```

### Medium Priority Test

```json
{
  "name": "Sara Ahmed",
  "email": "sara@example.com",
  "company": "XYZ Company",
  "service": "Web Development",
  "budget": 75000
}
```

Expected result:

```text
Score: 50
Priority: Medium
→ Google Sheets
→ Acknowledgment Email
```

### Low Priority Test

```json
{
  "name": "Usman Ali",
  "email": "usman@example.com",
  "company": "Small Business",
  "service": "Other",
  "budget": 30000
}
```

Expected result:

```text
Score: 20
Priority: Low
→ Nurture / Follow-up List
→ Acknowledgment Email
```

---

## Project Deliverables

The following files are included with this project:

```text
automated-lead-management/
│
├── automated-lead-management.json
├── sample-api-request.json
├── README.md
│
└── screenshots/
    ├── workflow.png
    ├── webhook.png
    ├── validation.png
    ├── lead-scoring.png
    ├── routing.png
    ├── google-sheets.png
    ├── sales-notification.png
    └── acknowledgment-email.png
```

---

## Key Learning Outcomes

Through this project, I practiced:

* Building webhook-based automations
* Validating incoming JSON data
* Processing and transforming lead information
* Writing JavaScript in n8n Code nodes
* Implementing lead-scoring logic
* Using IF and Switch nodes for conditional routing
* Integrating Google Sheets with n8n
* Integrating Gmail with n8n
* Testing webhooks using Postman
* Building a complete multi-step business automation workflow

---

## Conclusion

The Automated Lead Management System demonstrates how n8n can automate the complete lead-handling process from initial lead submission to scoring, routing, storage, sales notification, and customer acknowledgment.

The workflow reduces manual lead processing and ensures that leads are handled according to their priority automatically.
