
# HealthConnect Experience Lab — Week 8 Data Analytics & Decision Support Package

## 📌 Executive Summary
HealthConnect Clinic faced an active baseline no-show rate of **51.15%** across **4,737 active bookings** (excluding 263 canceled appointments), representing significant unrealized revenue and clinical capacity leakage. 

As part of the **Data Analytics (DA) Track**, this project established ground-truth operational KPIs, modeled key drivers of attendance friction, and built cross-track analytical assets to support predictive machine learning (DS) and automated communication routing (GenAI).

---

## 📊 Key Audited Metrics & Baseline KPIs

| Metric Category | Key Finding / Operational Metric | Business Impact |
| :--- | :--- | :--- |
| **Total Active Volume** | **4,737 Bookings** | Excludes 263 canceled records from active modeling |
| **Overall No-Show Rate** | **51.15%** (2,423 Missed Appointments) | Baseline operational capacity loss |
| **Extended Lead Time** | **63.95% No-Show Rate** for bookings >30 days out | 1,467 missed visits out of 2,294 active bookings |
| **Geographic Friction** | **70.42% No-Show Rate** for patients >30 km away | Highlights travel barriers for remote cohorts |
| **Critical Risk Cohort** | **61.76% Default Rate** (>30d lead & >30km distance) | High-friction compounding risk profile |
| **Repeat Absenteeism** | **47.54% Share of total no-shows** (1,152 visits) | Patients with $\ge 1$ prior missed visit |
| **Communication Uplift** | **SMS achieves 47.98% no-show rate** | **+6.65% attendance uplift** over unreminded bookings |

---

## 🛠️ Key Data Artifacts & Deliverables

1. **`HealthConnect_Executive_Dashboard_v2.pbix`**: Interactive Power BI dashboard featuring dynamic conditional formatting, centralized `_Measures` table, and parameter-driven cohort analysis.
2. **`HealthConnect_Final_Analytics_Report.docx`**: Executive decision-support package containing the complete implementation matrix and cross-track handoff specifications.
3. **`da_ds_feature_specs.csv`**: Data engineering specification file provided to the Data Science track. Includes zero-null imputed reminder channels and compound interaction features (`lead_time_x_distance_risk`) to optimize gradient boosting performance.

---

## 🧮 Refined DAX Measure Library

```dax
-- 1. Overall Active No-Show Rate
Overall_Active_No_Show_Rate = 
VAR ActiveTotal = 
    CALCULATE(
        COUNT('HealthConnect_Appointment_Data'[appointment_id]),
        'HealthConnect_Appointment_Data'[appointment_outcome] <> "Cancelled"
    )
VAR ActiveNoShows = 
    CALCULATE(
        COUNT('HealthConnect_Appointment_Data'[appointment_id]),
        'HealthConnect_Appointment_Data'[appointment_outcome] = "No-Show"
    )
RETURN
    DIVIDE(ActiveNoShows, ActiveTotal, 0)

-- 2. Critical Risk Cohort No-Show Rate (>30d Lead & >30km Distance)
Critical_Risk_No_Show_Rate = 
VAR TotalCriticalAppts = 
    CALCULATE(
        COUNT('HealthConnect_Appointment_Data'[appointment_id]),
        'HealthConnect_Appointment_Data'[appointment_outcome] <> "Cancelled",
        'HealthConnect_Appointment_Data'[booking_lead_days] > 30,
        'HealthConnect_Appointment_Data'[distance_to_clinic_km] > 30
    )
VAR MissedCriticalAppts = 
    CALCULATE(
        COUNT('HealthConnect_Appointment_Data'[appointment_id]),
        'HealthConnect_Appointment_Data'[appointment_outcome] = "No-Show",
        'HealthConnect_Appointment_Data'[booking_lead_days] > 30,
        'HealthConnect_Appointment_Data'[distance_to_clinic_km] > 30
    )
RETURN
    DIVIDE(MissedCriticalAppts, TotalCriticalAppts, 0)

```

---

## 💡 Strategic Recommendations & Implementation Matrix

* **Mandatory 48-Hour Re-confirmation**: Enforce confirmation for appointments booked >14 days in advance; auto-release unconfirmed slots to emergency waitlists to reduce idle capacity loss by up to 25%.
* **Universal Automated SMS Workflows**: Make SMS the default reminder channel across all bookings (T-72h, T-48h, T-24h) to capture the verified **+6.65% attendance uplift** (~180 slots saved/month).
* **High-Touch Escalation Protocol**: Implement direct administrative calls and pre-authorization policies for chronic absentees ($\ge 2$ prior no-shows) to address the **47.54% repeat default volume**.
* **Telehealth Consultation Routing**: Automatically offer virtual consultations during booking for patients residing $>30\text{ km}$ away to eliminate travel friction.

---

## 🤝 Cross-Track Collaboration Handoffs

* **Data Science (DS)**: Provisioned `da_ds_feature_specs.csv` with zero-null imputed reminder channels, reducing false-negative prediction errors by **3.2%** in high-distance cohorts.
* **Generative AI (GenAI)**: Supplied verified channel efficacy metrics (SMS outperforming WhatsApp by +4.76% and Email by +3.01%) to inform agent messaging escalation trees.
* **Project Management (PM)**: Delivered audited operational KPIs to ensure absolute alignment across capstone slide decks and stakeholder reports.

---

## 👨‍💻 Author

**Farouq Olanrewaju**

*Data Analyst | AnalystLab Africa Capstone Experience*

```

```
