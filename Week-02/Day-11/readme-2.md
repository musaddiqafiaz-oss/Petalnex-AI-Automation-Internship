# Day 11 — Lead Processing Workflow

## 1. Project Overview

This project demonstrates a multi-system automation workflow built using n8n.

The workflow receives a new lead through a Webhook, validates the incoming data, enriches the lead using an external API, stores the processed lead in Google Sheets, and sends a notification to the sales team through Gmail.

## 2. Objective

The objective of this workflow is to combine multiple systems into one automated pipeline.

The workflow demonstrates:

* Receiving data through a Webhook
* Validating incoming lead information
* Using an external API for data enrichment
* Storing lead information in Google Sheets
* Sending a notification through Gmail
* Connecting multiple systems into one workflow

## 3. Workflow

```text
New Lead
   ↓
Webhook
   ↓
Validate Lead
   ↓
API Lookup / Enrichment
   ↓
Edit Fields
   ↓
Google Sheets
   ↓
Gmail Notification
```

## 4. Technologies Used

* n8n
* Webhook
* HTTP Request
* REST Countries API
* IF Node
* Edit Fields Node
* Google Sheets
* Gmail
* Postman

## 5. Input Data

The workflow receives lead information through a POST Webhook.

Example:

```json
{
  "name": "Ali Khan",
  "email": "ali@gmail.com",
  "phone": "03001234567",
  "company": "ABC Ltd",
  "country": "Pakistan"
}
```

## 6. Workflow Steps

### Step 1 — Webhook

The Webhook node receives the new lead information through a POST request.

The test Webhook URL was used with Postman to send sample lead data.

### Step 2 — Validate

An IF node checks whether the required email field is available.

If the email is present, the lead continues through the workflow.

If the email is missing, the lead does not continue to the processing stage.

### Step 3 — API Lookup / Enrichment

The HTTP Request node sends the country information to the REST Countries API.

The API is used to retrieve additional information about the submitted country.

This demonstrates how external API data can be used to enrich incoming lead information.

### Step 4 — Edit Fields

The Edit Fields node organizes the original lead information and the API result into the required fields.

The fields include:

* Name
* Email
* Phone
* Company
* Country
* API Information
* Status

### Step 5 — Save Lead

The Google Sheets node uses the Append Row operation to save the processed lead.

The Google Sheet acts as the data store for the lead information.

### Step 6 — Notify Sales Team

After the lead is successfully saved, the Gmail node sends a notification to the sales team.

The notification contains the lead's basic information and confirms that a new lead has been received.

## 7. Sample Result

After successful execution, the lead is stored in Google Sheets.

Example:

| Name     | Email                                 | Phone       | Company | Country  | API Info | Status |
| -------- | ------------------------------------- | ----------- | ------- | -------- | -------- | ------ |
| Ali Khan | [ali@gmail.com](mailto:ali@gmail.com) | 03001234567 | ABC Ltd | Pakistan | Pakistan | New    |

A Gmail notification is also sent to notify the sales team.

## 8. Testing

The workflow was tested using Postman.

The following process was verified:

1. POST request sent to the n8n Webhook.
2. Lead data received successfully.
3. Required email field validated.
4. External API lookup completed.
5. Lead saved in Google Sheets.
6. Gmail notification sent successfully.

## 9. Deliverables

The following deliverables are included:

* Workflow JSON
* README file
* Screenshot of Webhook receiving data
* Screenshot of validation
* Screenshot of API lookup
* Screenshot of Google Sheets storing the lead
* Screenshot of Gmail notification
* Screenshot of the executiom

## 10. Conclusion

This project demonstrates how n8n can connect multiple systems into a single automated workflow.

The completed pipeline receives a lead, validates it, enriches the information using an external API, stores the lead in Google Sheets, and notifies the sales team through Gmail.

This workflow demonstrates practical multi-system integration and event-driven automation using n8n.
