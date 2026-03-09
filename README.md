# Road-Traffic-Fines-Process-Mining-Analysis
This project applies Process Mining techniques to analyze the Road Traffic Fines Management process using the Disco process mining tool. The analysis covers process discovery, conformance checking, time &amp; resource perspectives, and improvement recommendations.
### University of Vienna – Business Intelligence 1 | Assignment 2

**Submitted by:** Fahad Ali
## Project Overview

This project applies **Process Mining** techniques to analyze the **Road Traffic Fines Management** process using the [Disco](https://fluxicon.com/disco/) process mining tool. The analysis covers process discovery, conformance checking, time & resource perspectives, and improvement recommendations.

## 📂 Dataset

| Detail | Info |
|---|---|
| Dataset | Road Traffic Fines Management Event Log |
| Total Events | 561,470 |
| Total Cases | 150,370 |
| Activities | 11 |
| Time Range | Jan 2000 – Jun 2013 |
| Median Case Duration | 28.3 weeks |
| Mean Case Duration | 48.8 weeks |


## 🗂️ Project Structure

```
├── data/
│   └── road_traffic_fines.csv      # Event log (source dataset)
├── models/
│   └── bpmn_traffic_fine.bpmn      # BPMN model created in Disco
├── report/
│   └── BI_2_Report.pdf             # Full assignment report
└── README.md
```

---

## 🔍 Tasks Overview

### Task 1: Process Discovery
Used Disco to load and analyze the event log, progressively filtering the process map:

- **Phase 1:** Paths ~50%, Activities ~70% — main flow visible but still complex
- **Phase 2:** Paths ~40% + frequency filter (top 10% variants) — focused on mainstream behavior
- **Phase 3:** Paths ~10%, Activities ~30% — most streamlined view

A **BPMN model** was manually created to represent the ideal process flow, including:

| Activity | Description |
|---|---|
| Create Fine | Initiates the fine process |
| Send Fine | Sends fine to offender |
| Insert Fine Notification | Records & sends notification |
| Insert Appeal to Prefecture | Logs offender's appeal |
| Send Appeal to Prefecture | Forwards appeal to authority |
| Receive Result Appeal | Gets appeal outcome |
| Notify Result Appeal to Offender | Informs offender of decision |
| Payment | Manages fine payment |
| Send for Credit Collection | Escalates unpaid fines |
| Add Penalty | Adds penalties for late payment |

**Omitted from BPMN:** Appeal to Judge (rare exception), penalty steps that occur out of logical sequence.

---

### Task 2: Process Conformance

Three conformance violations were investigated:

**1. Fine paid before it is sent out**
- Filter: `Create Fine` → directly followed by → `Payment`
- Result: **46,880 cases** — significant financial integrity issue

**2. Payment reminder sent after fine already paid**
- Filter: `Payment` → directly followed by → `Insert Fine Notification`
- Result: **74 cases** — wasted resources and poor customer experience

**3. Appeal outcomes — re-assessed vs. cancelled**
- Total appeals: **532 cases**
- Re-assessed (payment made): **314 cases (59%)**
- Cancelled (fine cleared): **218 cases (41%)** — these were incorrectly routed to credit collection

---

### Task 3: Time & Resource Perspective

**Time Bottlenecks:**
- The mean (48.8 wks) is significantly higher than the median (28.3 wks), indicating long-tail outliers
- **"Send for Credit Collection"** (59,013 occurrences) is the primary bottleneck with very long waiting times
- **"Appeal to Judge"** takes ~30.6 months on average for just 0.1% of cases

**Resource Analysis (149 resources identified):**
- Resource **538** handles 64,097 events (29.81% of all events) — severely overutilized
- Median events per resource: 501
- Min events: 2 — significant workload imbalance across resources
- **"Insert Date Appeal to Prefecture"** takes 22.6 months for 39% of cases, suggesting resource bottleneck

---

### Task 4: Further Reflection

**Missing data that would improve analysis:**
- Detailed resource performance metrics
- Exception handling logs and descriptions
- Customer satisfaction/feedback data
- Reasons/comments per event (contextual attributes)
- Payment method details (online vs. in-person) and their effect on processing time

**Recommended Improvements:**
- Streamline the gap between "Add Penalty" and "Send for Credit Collection"
- Balance workload across resources — Resource 538 is overutilized
- Introduce automated data validation checks at point of entry
- Fix routing of cancelled appeals away from credit collection

---

## 🛠️ Tools Used

- **[Disco](https://fluxicon.com/disco/)** — Process mining and event log analysis
- **BPMN modeling** — Manual process model creation within Disco

---

## 🎓 Course Info

- **Course:** 2024S 052411-1 Business Intelligence 1
- **Institution:** University of Vienna
- **Assignment:** 2 — Process Mining
