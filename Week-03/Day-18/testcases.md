# Day 18 — Test Cases and Results

## Test Case 1 — Billing

### Input

```json
{
  "customer_name": "Ali Khan",
  "email": "ali.khan.test@example.com",
  "message": "My payment was deducted from my bank account, but my order is still showing as unpaid. Please check this issue."
}
```

### Result

```text
Category: Billing
Priority: High
Sentiment: Negative
Department: Billing
Summary: Customer's payment was deducted but the order is still showing as unpaid.
Suggested Response: The payment will be verified and the issue will be resolved.
```

---

## Test Case 2 — Technical

### Input

```json
{
  "customer_name": "Sara Ahmed",
  "email": "sara.ahmed.test@example.com",
  "message": "The website keeps crashing whenever I try to upload a file. I have tried several times but the problem is still happening."
}
```

### Result

```text
Category: Technical
Priority: High
Sentiment: Negative
Department: Technical Support
Summary: Customer is experiencing website crashes when uploading a file.
Suggested Response: The technical support team will investigate the upload issue.
```

---

## Test Case 3 — Account

### Input

```json
{
  "customer_name": "Hamza Khan",
  "email": "hamza.khan.test@example.com",
  "message": "I forgot my account password and I cannot log in. Please help me reset my password so I can access my account."
}
```

### Result

```text
Category: Account
Priority: Medium
Sentiment: Negative
Department: Customer Service
Summary: Customer cannot access the account because they forgot their password.
Suggested Response: The customer will be guided through the password reset process.
```

---

## Test Case 4 — Shipping

### Input

```json
{
  "customer_name": "Ayesha Ali",
  "email": "ayesha.ali.test@example.com",
  "message": "My package was supposed to arrive two days ago, but I still have not received it. Can you please check the delivery status?"
}
```

### Result

```text
Category: Shipping
Priority: High
Sentiment: Negative
Department: Shipping
Summary: Customer's package is delayed and has not arrived as expected.
Suggested Response: The delivery status will be checked and an update will be provided.
```

---

## Test Case 5 — Product

### Input

```json
{
  "customer_name": "Usman Raza",
  "email": "usman.raza.test@example.com",
  "message": "I recently purchased your product and would like to know how to use all of its main features. Can you provide some guidance?"
}
```

### Result

```text
Category: Product
Priority: Low
Sentiment: Neutral
Department: Customer Service
Summary: Customer needs guidance on using the main features of the product.
Suggested Response: The customer will be provided with guidance on the product's main features.
```

---

## Test Case 6 — Urgent Complaint

### Input

```json
{
  "customer_name": "Fatima Noor",
  "email": "fatima.noor.test@example.com",
  "message": "I am extremely angry! You charged me twice for the same order and nobody has helped me. I need this problem fixed immediately."
}
```

### Result

```text
Category: Complaint
Priority: Urgent
Sentiment: Angry
Department: Customer Service
Summary: Customer was charged twice and is frustrated because the issue has not been resolved.
Suggested Response: The issue will be investigated immediately and the duplicate charge will be reviewed.
```

### Urgent Alert

```text
Status: Triggered
```

---

## Test Case 7 — Positive / General

### Input

```json
{
  "customer_name": "Bilal Ahmed",
  "email": "bilal.ahmed.test@example.com",
  "message": "Thank you for solving my previous issue so quickly. I really appreciate the excellent support from your team."
}
```

### Result

```text
Category: General
Priority: Low
Sentiment: Positive
Department: Customer Service
Summary: Customer is thanking the support team for quickly resolving a previous issue.
Suggested Response: Thank the customer for their positive feedback and appreciation.
```

---

## Test Case 8 — Account Update

### Input

```json
{
  "customer_name": "Hira Khan",
  "email": "hira.khan.test@example.com",
  "message": "I recently changed my email address and need to update the email associated with my customer account. How can I do this?"
}
```

### Result

```text
Category: Account
Priority: Medium
Sentiment: Neutral
Department: Customer Service
Summary: Customer wants to update the email address associated with their account.
Suggested Response: The customer will be guided on how to update their account email address.
```

---

## Test Case 9 — Damaged Product

### Input

```json
{
  "customer_name": "Zain Ali",
  "email": "zain.ali.test@example.com",
  "message": "I received my order today, but unfortunately the product is damaged and cannot be used. I would like to know how I can get a replacement."
}
```

### Result

```text
Category: Product
Priority: High
Sentiment: Negative
Department: Customer Service
Summary: Customer received a damaged product and wants a replacement.
Suggested Response: The customer will be assisted with the replacement process.
```

---

## Test Case 10 — Service Plans

### Input

```json
{
  "customer_name": "Maria Khan",
  "email": "maria.khan.test@example.com",
  "message": "I am interested in your services and would like more information about the plans you currently offer, including their features and pricing."
}
```

### Result

```text
Category: General
Priority: Low
Sentiment: Neutral
Department: Sales
Summary: Customer is requesting information about available service plans, features, and pricing.
Suggested Response: The available service plans, features, and pricing will be provided to the customer.
```

---

## Test Summary

| Test | Category  | Priority | Main Result                  |
| ---- | --------- | -------- | ---------------------------- |
| 1    | Billing   | High     | Ticket stored                |
| 2    | Technical | High     | Ticket stored                |
| 3    | Account   | Medium   | Ticket stored                |
| 4    | Shipping  | High     | Ticket stored                |
| 5    | Product   | Low      | Ticket stored                |
| 6    | Complaint | Urgent   | Ticket stored + urgent alert |
| 7    | General   | Low      | Ticket stored                |
| 8    | Account   | Medium   | Ticket stored                |
| 9    | Product   | High     | Ticket stored                |
| 10   | General   | Low      | Ticket stored                |

**Total Test Cases: 10**

**Urgent Alert Tests: 1**

**All tests were executed through the n8n customer support triage workflow.**
