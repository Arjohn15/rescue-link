# Automated Triage Algorithm

## Overview

RescueLink uses a Lexicon-Based Weighted Scoring Algorithm to automatically analyze rescue request messages and determine the needs and urgency of each request. The algorithm runs on the backend (NestJS) immediately after a request is submitted and before the request is stored in the database.

---

## Approach

### Lexicon-Based Analysis

A predefined lexicon maps keywords and phrases to specific needs and urgency weights. When a message is received, the algorithm scans it for matching terms and accumulates a weighted score.

### Weighted Scoring

Each keyword is assigned a weight based on its urgency level:

| Weight | Urgency  | Example Keywords                                               |
| ------ | -------- | -------------------------------------------------------------- |
| 3      | Critical | unconscious, not breathing, severe bleeding, trapped, drowning |
| 2      | High     | injured, sick, no food, flooded, elderly, infant, disabled     |
| 1      | Moderate | need help, water, shelter, stranded, cold                      |

### Triage Score

The final triage score is the sum of all matched keyword weights. A higher score means higher priority. The score is stored in the `triage_score` column of the `rescue_requests` table.

### Suggested Needs

Based on matched keywords, the algorithm maps results to the predefined need categories:

| Need       | Trigger Keywords                                                    |
| ---------- | ------------------------------------------------------------------- |
| `medical`  | unconscious, injured, sick, bleeding, pain, not breathing, disabled |
| `food`     | no food, hungry, starving                                           |
| `water`    | no water, thirsty, flood, drowning                                  |
| `shelter`  | no shelter, homeless, stranded                                      |
| `rescue`   | trapped, stuck, cannot move, drowning                               |
| `clothing` | no clothes, cold                                                    |

---

## Flow

```
Resident submits message
        ↓
Backend receives request
        ↓
Triage algorithm scans message against lexicon
        ↓
Computes triage_score and suggested_needs
        ↓
Stores results in rescue_requests table
        ↓
Admin dashboard displays triage_score and suggested_needs
        ↓
Admin can override suggested_needs if needed
```

---

## Admin Override

Admins can override the suggested needs from the request details view. The overridden needs replace the algorithm's suggestions and are stored as the final confirmed needs for the request. The triage score remains unchanged after an override since it reflects the original message analysis.

---

## Database Fields

| Field             | Table             | Description                                |
| ----------------- | ----------------- | ------------------------------------------ |
| `triage_score`    | `rescue_requests` | Computed weighted score from the algorithm |
| `suggested_needs` | `rescue_requests` | Auto-detected needs from lexicon analysis  |

---

## Notes

- The algorithm runs entirely on the backend (NestJS) and is not exposed to the frontend directly.
- The lexicon can be expanded over time to improve detection accuracy.
- Future improvement: replace lexicon-based approach with an NLP or ML model for higher accuracy.
