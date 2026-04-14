## Calculation Glossary

### Completed Task Count  
The total number of tasks with a task status of **Completed**.

---

### Standard Handle Time  
The pre-determined amount of time that a single task, multiplied by work items (if applicable), is expected to take.

This value is established annually based on:
- An analysis of the prior twelve months of historical data for each task, grouped by:
  - Business Unit  
  - Business Team  
  - Task Level  
  - Task Type  
- Feedback/Signoff from each leadership group using the tool is required.

---

### Random Multiplier  
A randomly generated numeric factor used to obfuscate results so they cannot be used for time-based individual performance evaluation.

---

### Weighted Task Count  
A normalized volume metric that adjusts completed tasks by expected time and a random multiplier.

**Base Formula (individual/contributor level):**  
`Weighted Task Count = (Completed Task Count × Standard Handle Time) ÷ Random Multiplier`

**Leader-Level Adjustment:**  
When viewed at a leader level, the result is further divided by the number of downstream representatives.

Example:  
If a manager has 12 direct reports, then:  
`Leader Weighted Task Count = Weighted Task Count ÷ 12`

---

### Average Handle Time (AHT)  
The average amount of in-progress working time spent on completed tasks.

**Formula:**  
`Average Handle Time (AHT) = Total In-Progress Time on Completed Tasks ÷ Completed Task Count`

---

### Average Handle Time Variance  
The variance between the actual average handle time and the standard handle time.

**Formula:**  
`Average Handle Time Variance = Average Handle Time (AHT) − Standard Handle Time`

---

### Paused Count  
The total number of times a **Completed** task entered a **Paused** status.

---

### In Progress Count  
The total number of times a **Completed** task entered an **In Progress** status.

---

### Paused Rate  
The proportion of Paused events relative to all Paused and In Progress events for completed tasks.

**Formula:**  
`Paused Rate = Paused Count ÷ (Paused Count + In Progress Count)`

---

### Abandoned Count  
The total number of times a task was assigned an **Abandoned** status before reaching a **Completed** status.

---

### Abandoned Rate  
The proportion of outcomes that were Abandoned relative to all non-completed outcome dispositions.

**Formula:**  
`Abandoned Rate = Abandoned Count ÷ (Abandoned Count + Already Completed Count + Reassigned Count + Rejected Count + Other Count)`

---

### Already Completed Count  
The total number of times a task was found to be **Already Completed** when opened.

---

### Already Completed Rate  
The proportion of outcomes that were Already Completed relative to all non-completed outcome dispositions.

**Formula:**  
`Already Completed Rate = Already Completed Count ÷ (Abandoned Count + Already Completed Count + Reassigned Count + Rejected Count + Other Count)`

---

### Reassigned Count  
The total number of times a task was **Reassigned** before reaching a **Completed** status.

---

### Reassigned Rate  
The proportion of outcomes that were Reassigned relative to all non-completed outcome dispositions.

**Formula:**  
`Reassigned Rate = Reassigned Count ÷ (Abandoned Count + Already Completed Count + Reassigned Count + Rejected Count + Other Count)`

---

### Rejected Count  
The total number of times a task was **Rejected** before reaching a **Completed** status.

---

### Rejected Rate  
The proportion of outcomes that were Rejected relative to all non-completed outcome dispositions.

**Formula:**  
`Rejected Rate = Rejected Count ÷ (Abandoned Count + Already Completed Count + Reassigned Count + Rejected Count + Other Count)`

---

### Other Count  
The total number of times a task was given an **Other** (unknown or uncategorized) status before reaching a **Completed** status.

---

### Other Rate  
The proportion of outcomes that were classified as Other relative to all non-completed outcome dispositions.

**Formula:**  
`Other Rate = Other Count ÷ (Abandoned Count + Already Completed Count + Reassigned Count + Rejected Count + Other Count)`
