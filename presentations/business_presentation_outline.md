# Business Presentation Outline

## Slide 1 — Catching Student Dropout Risk Earlier

### A Two-Stage Early-Warning Strategy for More Targeted Student Support

**The opportunity**

Identify students who may need support at two critical moments:

**Enrollment**
Earlier signal → more time to act

**Semester 1**
Stronger signal → more precise intervention

**Core principle**
Use predictions to prioritize supportive outreach—not to automate decisions about students.

**Key message:**  
Earlier data gives us more time. Semester 1 data gives us more confidence.

---

## Slide 2 — The Business Problem

### Student support is limited. Timing matters.

Institutions face three practical challenges:

- Students at risk may not be identified until intervention opportunities have already narrowed.
- Support teams have limited time, staffing, and intervention capacity.
- Poor targeting means resources may go to students who do not need intensive support while other at-risk students are missed.

### Business Question

**Can we identify students who may need support early—and improve targeting as better information becomes available?**

### What success means

Not simply having an accurate model.

Success means:

**Catch more at-risk students + reduce unnecessary interventions + act early enough to make support possible.**

**Key message:**  
The problem is ultimately about allocating limited student-support resources more effectively.

---

## Slide 3 — The Solution: Two Intervention Moments

### Stage 1 — Enrollment

Use information available when the student enters the institution.

**Best suited for:**
- onboarding support
- adviser check-ins
- resource awareness
- broad, low-cost outreach

**Advantage:** Maximum lead time  
**Trade-off:** More false alarms

### Stage 2 — Semester 1

Refresh risk using actual first-semester academic progress.

**Best suited for:**
- targeted advising
- tutoring referrals
- academic recovery planning
- higher-touch intervention

**Advantage:** Much stronger targeting  
**Trade-off:** Intervention occurs later

**Key message:**  
The institution does not need to choose between early and accurate prediction. Each model serves a different intervention purpose.

---

## Slide 4 — How Much Better Does Targeting Get?

### Held-Out Test Performance

| Measure | Enrollment | Semester 1 |
|---|---:|---:|
| Dropout Recall | **72.2%** | **77.5%** |
| Precision | **52.6%** | **69.8%** |
| F1 Score | **60.8%** | **73.5%** |

### What changes after Semester 1?

**+15**
more students labeled Dropout correctly identified

**−90**
fewer false-positive alerts

**−15**
fewer missed Dropout cases

### In student counts

Enrollment:
- 205 correctly identified Dropout cases
- 79 missed
- 185 false-positive alerts

Semester 1:
- 220 correctly identified Dropout cases
- 64 missed
- 95 false-positive alerts

**Key message:**  
Semester 1 information improves both detection and efficiency—it catches more at-risk students while generating far fewer unnecessary alerts.

---

## Slide 5 — What Changes When We Know More?

### At Enrollment, the model relies more on:

- scholarship status
- course
- application mode
- admission grade
- age and other background characteristics

### After Semester 1, the model shifts toward:

- approved units
- Semester 1 grades
- enrolled units
- approval rate
- completion progress

### Why this matters

The later model increasingly relies on **what students are actually experiencing academically**, rather than mainly on characteristics known when they entered.

**Key message:**  
Semester 1 converts the system from primarily background-based risk screening into academic-progress-based early warning.

---

## Slide 6 — What If We Can Support Only 10% of Students?

### Simulated intervention capacity

Held-out test population: **885 students**

Available intervention slots: **89 students**

| Stage | Students Prioritized | Dropout Cases Reached | Precision |
|---|---:|---:|---:|
| Enrollment | 89 | **73** | **82.0%** |
| Semester 1 | 89 | **88** | **98.9%** |

### Operational interpretation

At Enrollment:

**82 of every 100 students prioritized** would be expected to belong to the Dropout class in this held-out sample.

After Semester 1:

Approximately **99 of every 100 students prioritized** belonged to the Dropout class.

**Key message:**  
When support capacity is constrained, Semester 1 risk ranking produces a highly concentrated intervention group.

**Important:**  
These figures come from the current held-out test sample and must be validated on future cohorts before operational use.

---

## Slide 7 — Turning Prediction Into an Intervention Strategy

### Enrollment → Broad, Low-Cost Support

Use the earlier signal where the cost of a false positive is low.

Examples:

- automated resource reminders
- adviser introductions
- onboarding nudges
- study-support awareness
- general check-ins

### Semester 1 → Targeted, Higher-Touch Support

Use the stronger signal where interventions require more resources.

Examples:

- individual academic advising
- tutoring referrals
- progress reviews
- academic recovery planning
- proactive outreach

### Recommended operating model

**Screen early → monitor progress → refresh risk → escalate support**

**Key message:**  
Risk scoring becomes more valuable when it is connected to different levels of intervention intensity.

---

## Slide 8 — Responsible AI: The Risk We Cannot Ignore

### The models do not perform equally across all groups.

Observed Dropout Recall gaps:

| Group | Enrollment Gap | Semester 1 Gap |
|---|---:|---:|
| Gender | **29.1 pts** | **9.7 pts** |
| Age | **45.4 pts** | **27.7 pts** |
| Scholarship Status | **65.0 pts** | **40.2 pts** |

Semester 1 reduces each observed gap—but does not eliminate them.

### Required safeguards

- Human review before intensive intervention decisions
- Monitor missed-risk rates by subgroup
- Validate on future cohorts
- Test fairness-mitigation approaches
- Review thresholds carefully
- Never use risk scores for punitive decisions

**Key message:**  
A useful model can still produce unequal error patterns. Fairness monitoring must be part of operations, not an afterthought.

---

## Slide 9 — Business Value & ROI Framework

### What the model already demonstrates

Moving from Enrollment to Semester 1:

**+15**
additional Dropout cases correctly identified

**−90**
false-positive interventions avoided

**69.8%**
precision versus 52.6% at Enrollment

**88 of 89**
top-risk intervention slots correspond to Dropout cases in the held-out sample

### Translating this into financial value

The dataset does not contain institutional tuition value, support costs, or intervention success rates, so monetary ROI should not be invented.

Instead, use:

### Illustrative ROI Framework

**Expected students retained**

`= At-risk students reached × intervention success rate`

**Retention value**

`= Expected students retained × value of retaining one student`

**Avoided outreach cost**

`= Unnecessary interventions avoided × cost per intervention`

**Estimated net value**

`= Retention value + avoided outreach cost − total program cost`

### Inputs required from the institution

| Business Input | Institution Provides |
|---|---:|
| Value of retaining one student | ₱X |
| Cost per intervention | ₱Y |
| Intervention success rate | Z% |
| Available intervention capacity | N students |
| Program operating cost | ₱P |

### Recommended Business KPIs

- Dropout cases reached
- Intervention precision
- False positives avoided
- Cost per successfully supported student
- Retention uplift after intervention
- Net retention value
- Fairness gaps across key student groups

**Key message:**  
The business case is not “AI saves ₱X.” The value comes from reaching more genuinely at-risk students while reducing wasted intervention capacity.

---

## Slide 10 — Recommendation: Use Both Models

### Adopt a Two-Stage Early-Warning System

**1 — Enrollment**

Use broad risk screening for early, low-cost support.

**2 — Semester 1**

Refresh risk with academic-progress data and prioritize limited high-touch interventions.

### Before real-world deployment

**Validate**
Test performance on future student cohorts.

**Operationalize**
Define intervention capacity, costs, and thresholds.

**Protect**
Establish human-review and responsible-use policies.

**Monitor**
Track predictive performance and subgroup disparities over time.

**Improve**
Test fairness-mitigation strategies before changing operational rules.

### Final Recommendation

**Use the Enrollment model to buy time.  
Use the Semester 1 model to improve confidence.  
Use human judgment to decide how support is delivered.**

### Final takeaway

> **Earlier data gives us more time. Semester 1 data gives us more confidence. The strongest student-support strategy uses both.**
