# hospital-dashboard
An ETL pipeline that extracts csv data, performs necessary transformation and feature engineering, and loads to a SQL database to be used as possible source for hospital operations dashboard.

## Project Overview
In hospitals, operational dashboards are needed to see turn-over times of beds, length of stay of patients, revenue per case, and departments handling the cases in order to see a broader overview of hospital performance. This allows the management to forecast where they can improve on services and decision-making. 

For this operational dashboard, we used synthetically generated patient encounter data that mimics the transactional data produced by real-world Hospital Information Systems (HIS) and Electronic Health Records (EHR) systems. We loaded the generated data locally, constituting the extract phase, performed transformations and feature engineering for the transform phase, and loaded the data finally into a SQL database. 

---

## 💾 Dashboard Data & Dimensional Model
The foundation of this dashboard is a star schema designed for efficient filtering and fast calculation of key performance indicators (KPIs). The data represents 10 patient encounters across three separate Tenet Healthcare facilities during November 2025.

**The Fact Table (Metrics)**
The central table is FACT_PATIENT_ENCOUNTER, which contains all the metrics (measures) that can be aggregated and calculated:
1. **Financial Metrics**: Total_Charges, Revenue_Per_Case.
2. **Operational Metrics**: Admit_Timestamp, Discharge_Timestamp, LOS_Hours.
3. **Quality Metrics**: Readmission_Flag, Is_ED_Admit.
4.**Derived Metrics**: Encounter_Status (calculated based on the discharge time to determine current census).

**The Dimension Tables (Context)**
Three dimension tables provide the context (attributes) needed to filter and slice the fact data:
1. **DIM_HOSPITAL**: Provides the context for facility-level analysis, including Region and the critical Staffed_Beds count, which is necessary for calculating Bed Occupancy Rate.
2. **DIM_PATIENT**: Contains demographic context, including the derived Age_Group and patient Payer_Group for financial and utilization analysis.
3. **DIM_DATE**: Provides detailed time context, allowing for analysis by Day_of_Week, Is_Weekend, and supporting time intelligence (e.g., comparing ALOS to the prior period).

This dimensional model ensures that managers can quickly analyze KPIs across multiple dimensions, such as "What is the Average Length of Stay for Cardiology patients with a Medicare payer in the Southwest Region?"

---
In the end, the end-product is a Data Mart, readily available tables that can be used as source for the hospital operations dashboard. One can use Power BI to make the necessary schema connections and DAX measures, then layout the whole dashboard. 
