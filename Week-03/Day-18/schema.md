{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CustomerSupportTriageOutput",
  "description": "Structured output the AI Analysis step must return for every customer support message.",
  "type": "object",
  "properties": {
    "category": {
      "type": "string",
      "enum": ["Billing", "Technical", "Account", "Shipping", "Product", "Complaint", "General"],
      "description": "Primary topic of the customer's message."
    },
    "priority": {
      "type": "string",
      "enum": ["Low", "Medium", "High", "Urgent"],
      "description": "How quickly the ticket needs a response. 'Urgent' triggers the internal alert email."
    },
    "sentiment": {
      "type": "string",
      "enum": ["Positive", "Neutral", "Negative", "Angry"],
      "description": "Customer's emotional tone."
    },
    "department": {
      "type": "string",
      "enum": ["Billing", "Technical Support", "Customer Service", "Shipping", "Sales"],
      "description": "Team responsible for handling the ticket."
    },
    "summary": {
      "type": "string",
      "minLength": 1,
      "maxLength": 400,
      "description": "Concise, factual summary of the customer's issue (1-2 sentences)."
    },
    "suggested_response": {
      "type": "string",
      "minLength": 1,
      "description": "A professional, ready-to-send draft reply to the customer."
    }
  },
  "required": ["category", "priority", "sentiment", "department", "summary", "suggested_response"],
  "additionalProperties": false
}