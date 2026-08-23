# Day 9 — JavaScript for Automation (the Code Node)

## Objective
Learn just enough practical JavaScript to transform data that nodes alone can't, using n8n's Code node to normalize messy records and filter them based on computed values.

## Workflow Overview

```
When clicking 'Execute workflow'  →  Code in JavaScript (transform)  →  Code in JavaScript1 (filter)
```

- **Node 1 — Code in JavaScript**: normalizes 15 raw records (title-case names, lowercase/trim emails) and computes a `grade` field from `score` using `.map()`.
- **Node 2 — Code in JavaScript1**: keeps only records with `grade` A or B using `.filter()`.

File: `javascript_for_automation.json` (importable directly into n8n via **Import from File**).

## Code Node Scripts

### Node 1 — Transform (Run Once for All Items)

```javascript
const records = [
  { name: "  ali KHAN ", email: "ALI.KHAN@GMAIL.COM ", score: 92 },
  { name: " sara ahmed", email: "Sara.Ahmed@GMAIL.COM", score: 85 },
  { name: "HAMZA malik ", email: " HAMZA.MALIK@GMAIL.COM", score: 76 },
  { name: "  ayesha IQBAL", email: "AYESHA.IQBAL@GMAIL.COM", score: 68 },
  { name: "usman RAZA", email: "Usman.Raza@GMAIL.COM", score: 91 },
  { name: "fatima khan ", email: "FATIMA.KHAN@GMAIL.COM", score: 73 },
  { name: "  zain ALI", email: "zain.ali@GMAIL.COM", score: 88 },
  { name: "HINA shah", email: "HINA.SHAH@GMAIL.COM", score: 59 },
  { name: "bilal ahmad ", email: "BILAL.AHMAD@GMAIL.COM", score: 81 },
  { name: "  maria iqbal ", email: "Maria.Iqbal@GMAIL.COM", score: 95 },
  { name: "DANIYAL KHAN", email: "DANIYAL.KHAN@GMAIL.COM", score: 64 },
  { name: "sana malik ", email: "SANA.MALIK@GMAIL.COM", score: 87 },
  { name: "  hamid ali", email: "HAMID.ALI@GMAIL.COM", score: 70 },
  { name: "noor FATIMA ", email: "Noor.Fatima@GMAIL.COM", score: 90 },
  { name: "  talha ahmed", email: "TALHA.AHMED@GMAIL.COM", score: 55 }
];

const normalized = records.map(record => {
  const name = record.name
    .trim()
    .toLowerCase()
    .split(/\s+/)
    .map(word => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");

  const email = record.email.trim().toLowerCase();

  let grade;

  if (record.score >= 90) {
    grade = "A";
  } else if (record.score >= 80) {
    grade = "B";
  } else if (record.score >= 70) {
    grade = "C";
  } else if (record.score >= 60) {
    grade = "D";
  } else {
    grade = "F";
  }

  return {
    name,
    email,
    score: Number(record.score),
    grade
  };
});

return normalized.map(record => ({
  json: record
}));
```

### Node 2 — Filter (Run Once for All Items)

```javascript
const filtered = $input.all().filter(item => {
  return item.json.grade === "A" || item.json.grade === "B";
});

return filtered;
```

## Before / After Sample Data

**Before (raw input, sample of 3 of 15):**
```json
[
  { "name": "  ali KHAN ", "email": "ALI.KHAN@GMAIL.COM ", "score": 92 },
  { "name": " sara ahmed", "email": "Sara.Ahmed@GMAIL.COM", "score": 85 },
  { "name": "HAMZA malik ", "email": " HAMZA.MALIK@GMAIL.COM", "score": 76 }
]
```

**After Node 1 (normalized + graded, all 15):**
```json
[
  { "name": "Ali Khan",     "email": "ali.khan@gmail.com",     "score": 92, "grade": "A" },
  { "name": "Sara Ahmed",   "email": "sara.ahmed@gmail.com",   "score": 85, "grade": "B" },
  { "name": "Hamza Malik",  "email": "hamza.malik@gmail.com",  "score": 76, "grade": "C" },
  { "name": "Ayesha Iqbal", "email": "ayesha.iqbal@gmail.com", "score": 68, "grade": "D" },
  { "name": "Usman Raza",   "email": "usman.raza@gmail.com",   "score": 91, "grade": "A" },
  { "name": "Fatima Khan",  "email": "fatima.khan@gmail.com",  "score": 73, "grade": "C" },
  { "name": "Zain Ali",     "email": "zain.ali@gmail.com",     "score": 88, "grade": "B" },
  { "name": "Hina Shah",    "email": "hina.shah@gmail.com",    "score": 59, "grade": "F" },
  { "name": "Bilal Ahmad",  "email": "bilal.ahmad@gmail.com",  "score": 81, "grade": "B" },
  { "name": "Maria Iqbal",  "email": "maria.iqbal@gmail.com",  "score": 95, "grade": "A" },
  { "name": "Daniyal Khan", "email": "daniyal.khan@gmail.com", "score": 64, "grade": "D" },
  { "name": "Sana Malik",   "email": "sana.malik@gmail.com",   "score": 87, "grade": "B" },
  { "name": "Hamid Ali",    "email": "hamid.ali@gmail.com",    "score": 70, "grade": "C" },
  { "name": "Noor Fatima",  "email": "noor.fatima@gmail.com",  "score": 90, "grade": "A" },
  { "name": "Talha Ahmed",  "email": "talha.ahmed@gmail.com",  "score": 55, "grade": "F" }
]
```

**After Node 2 (final filtered output, grade ≥ B — 8 of 15 records):**
```json
[
  { "name": "Ali Khan",    "email": "ali.khan@gmail.com",    "score": 92, "grade": "A" },
  { "name": "Sara Ahmed",  "email": "sara.ahmed@gmail.com",  "score": 85, "grade": "B" },
  { "name": "Usman Raza",  "email": "usman.raza@gmail.com",  "score": 91, "grade": "A" },
  { "name": "Zain Ali",    "email": "zain.ali@gmail.com",    "score": 88, "grade": "B" },
  { "name": "Bilal Ahmad", "email": "bilal.ahmad@gmail.com", "score": 81, "grade": "B" },
  { "name": "Maria Iqbal", "email": "maria.iqbal@gmail.com", "score": 95, "grade": "A" },
  { "name": "Sana Malik",  "email": "sana.malik@gmail.com",  "score": 87, "grade": "B" },
  { "name": "Noor Fatima", "email": "noor.fatima@gmail.com", "score": 90, "grade": "A" }
]
```

## Key Concepts Applied

| Concept | Where used |
|---|---|
| `.trim()` / `.toLowerCase()` | Cleaning name & email whitespace/casing |
| `.split()` + `.map()` + `.join()` | Title-casing multi-word names |
| `if / else if / else` | Grade band assignment (A–F) |
| `.map()` | Transforming all 15 raw records into normalized records |
| `.filter()` | Keeping only grade A/B records in Node 2 |
| `$input.all()` | Reading items into a Code node from the previous node |
| Run Once for All Items | Both nodes process the full item array in one execution |

## How to Run
1. Import `javascript_for_automation.json` into n8n.
2. Click **Execute workflow**.
3. Open **Code in JavaScript** → check output tab: 15 items, normalized + graded.
4. Open **Code in JavaScript1** → check output tab: 8 items, grade A/B only.
