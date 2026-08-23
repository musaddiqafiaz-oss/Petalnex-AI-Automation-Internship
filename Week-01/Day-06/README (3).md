# Candidate Screening Automation

## Objective
This project automates the candidate screening process using n8n. It reads candidate information from Google Sheets, validates required fields, categorizes candidates based on screening rules, updates the candidate status in Google Sheets, and sends an appropriate email notification.

## Workflow
Manual Trigger
↓
Google Sheets (Read Rows)
↓
IF (Validate Required Fields)
↓
IF (Qualified?)
↓
IF (Needs Review?)
↓
Update Google Sheets
↓
Send Gmail Notification

## Screening Rules

### Qualified
- Degree contains "BS"
- Skills contain "Python"
- Experience is 1 years or more

### Needs Review
- Degree contains "BS"
- Experience is at least 1 year
- Does not meet all Qualified conditions

### Rejected
- Does not meet the above criteria

### Invalid
- Missing required fields (Email, Degree, or Skills)

## Technologies Used
- n8n
- Google Sheets
- Gmail

## Sample Data
The workflow uses candidate data stored in a Google Sheet with the following columns:
- Name
- Email
- Degree
- Skills
- Experience
- Availability
- Status

## Result
The workflow successfully:
- Reads candidate records from Google Sheets.
- Validates required information.
- Categorizes candidates.
- Updates the Status column.
- Sends an appropriate email to each candidate.