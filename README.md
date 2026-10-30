# Patient-No-Show-Analysis-for-Healthcare-Data
This repository contains an end-to-end SQL &amp; Tableau project designed to understand the root causes of missed visits in order to cut financial and operational waste.

## Dataset
https://drive.google.com/file/d/1Mp_zax1N6RITrRBMzWed6CO3M-9ed7YK/view?usp=sharing

## Project Pipeline & Methodology
The project follows an end-to-end data analytics lifecycle, transitioning raw operational records into actionable patient risk intelligence through a structured multi-stage pipeline:

* **Data Ingestion & Setup:** Created a relational database schema (healthcare_analytics) in MySQL Workbench to house medical appointment logs and patient demographic records.
* **Data Cleaning & Standardization:**
    1. Renamed non-standard column headers (e.g., Hipertension $\rightarrow$ hypertension, Handcap $\rightarrow$ disability_count, and No-show $\rightarrow$ no_show) to maintain clean SQL querying standards.
    2. Parsed text-based timestamps (ScheduledDay, AppointmentDay) into proper SQL DATETIME and DATE types using string replacement and STR_TO_DATE.
    3. Filtered out data entry errors and anomalies, such as negative ages (Age = -1) and invalid booking intervals (lead_time_days < 0).
* **Feature Engineering:**
    1. Calculated appointment lead_time_days using date-difference logic to evaluate the impact of advance scheduling on attendance.
    2. Segmented continuous variables into meaningful operational groups, such as age tiers (Child, Teen, Young Adult, Adult, Senior) and lead-time buckets (Same Day, Short [1–3 Days], Within a Week, Long Lead [8+ Days]).
* **Advanced Window Modeling & Risk View Creation:**
    1. Leveraged window functions (PARTITION BY PatientID ORDER BY AppointmentDay) to dynamically compute historical metrics (prior_appointments and prior_no_shows) prior to each visit.
    2. Encapsulated this logic into a reusable SQL view (v_appointment_risk), classifying patient visits into explicit risk tiers (High Risk, Medium Risk, Low Risk, and New Patient - Monitor) based on historical no-show rates and booking lead times.
* **BI Dashboard Integration:** Exported and connected the cleaned relational datasets and risk views to Tableau Desktop to build an interactive operations command center.   

## Key Technical Stack
* **Database Management & SQL:** MySQL Workbench, MySQL (DDL/DML operations, complex CTEs, window functions OVER(), views, date manipulation functions).
* **Data Visualization & BI:** Tableau Desktop (Calculated fields, custom aggregations, multi-source data relationships, dynamic filtering, interactive dashboard layouts).
* **Version Control & Portfolio Presentation:** Markdown, GitHub repository management.

## Insights and Next Steps
 ### Key Insights
* **Lead Time Impact:** Appointments booked far in advance (Long Lead 8+ days) exhibit significantly higher no-show rates compared to same-day or short-notice bookings.
* **Patient History Predictability:** Patients with a repeating pattern of prior missed appointments drive a disproportionate share of total operational waste, making the v_appointment_risk view a powerful predictive asset.
* **Geographic Concentration:** Analysis across neighborhoods highlights specific high-risk zones (captured via the Top 15 Highest-Rank Neighborhoods view) where targeted community outreach can be deployed.
* **Operational Overview:** High-level KPIs tracked via the Tableau summary cards (Total Appointments, No-Show Rate, and AVG Lead Time) establish a clear operational baseline for clinic managers.

### Recommended Next Steps
* **Automated Intervention Triggers:** Integrate the SQL v_appointment_risk view directly with clinic messaging systems to automatically trigger personalized, multi-channel reminders (SMS and phone calls) specifically for patients flagged as High Risk.
* **Predictive Machine Learning Expansion:** Upgrade the heuristic SQL risk-tier logic into a machine learning classification model (e.g., XGBoost or Random Forest) using Python (Scikit-Learn) to predict no-show probabilities at the exact moment an appointment is booked.
* **Smart Scheduling Rules:** Advise clinic administrators to optimize booking workflows—such as capping maximum advance booking windows for high-risk patient cohorts or implementing overbooking strategies backed by historical data trends.

## Dashboard



 
