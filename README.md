# Healthcare Analysis Dashboard | Power BI

An interactive 3-page Power BI report that analyses patient admissions, billing and hospital performance, with synced slicers, page navigation and a drill-through page for hospital-level detail.

<img width="907" height="506" alt="healthcare_powerbi_page1 jpg" src="https://github.com/user-attachments/assets/8e714480-fb59-4614-a81b-abe1ea9eb73e" />

---

## Objective

- Understand patient volume by month, gender, medical condition and admission type.
- Measure revenue and average billing by insurance provider, medical condition and hospital.
- Give users one report where every filter works across all pages, and one click to drill into a single hospital.

## Dataset

| Item | Detail |
|---|---|
| Source | [Kaggle - Healthcare Dataset](https://www.kaggle.com/datasets/prasad22/healthcare=dataset) (synthetic data) |
| Size | 55,500 rows and 15 columns (about 54,964 patient records after removing duplicates) |
| Period | May 2019 to May 2024 |
| Key columns | Age, Gender, Blood Type, Medical Condition, Date of Admission, Discharge Date, Doctor, Hospital, Insurance Provider, Billing Amount, Room Number, Admission Type, Medication, Test Results |

**Added during the project:** a Calendar table, age group, year and month columns, and the DAX measures listed below.

## Tools Used

- **Power BI Desktop** - data model, visuals, slicers, navigation, drill through
- **DAX** - KPI measures, percentages, year-over-year revenue
- **Power Query** - data cleaning (duplicates, data types, name formatting)
- **Excel / CSV** - source data
- **GitHub** - documentation and portfolio

## Dashboard Pages

### Page 1 - Patient Review
KPI cards (Total Patients, Total Revenue, Avg Billing, Avg Stay), admissions by month (2019-2024), gender split, medical conditions by patients, and admission type.

<img width="907" height="506" alt="healthcare_powerbi_page1 jpg" src="https://github.com/user-attachments/assets/9c4f9d2f-d888-4420-bcb0-8de9db375827" />


### Page 2 - Revenue and Insights
Revenue by insurance provider, top 10 hospitals by revenue, average billing by medical condition, a medication-by-condition matrix, and a revenue treemap by condition.

<img width="925" height="507" alt="healthcare_powerbi_page(2) jpg" src="https://github.com/user-attachments/assets/a0e89c35-c799-4729-b4b5-18e2f74c2c4d" />


### Page 3 - Hospital Details (drill through)
Right-click a hospital on page 2 and choose **Drill through > Hospital_details**. The page shows the selected hospital's name, KPIs, conditions and admission types.

<img width="922" height="503" alt="Healthcare_powerbi_page(3) jpg" src="https://github.com/user-attachments/assets/d83f736e-00ce-462a-889f-2ba7ed251fe9" />


## Key Features

- **Synced slicers** - Year, Medical Condition, Gender, Insurance Provider, Hospital and Admission Type filter all pages together.
- **Page navigation** - sidebar buttons to move between pages.
- **Drill through** - hospital-level detail page, with a dynamic hospital name.
- **KPI cards** on every page.
- **Consistent design** - one colour theme (navy and blue), key-insight text boxes on each page.

## Key Insights

1. Patients are split almost equally by gender: about **27.5K male and 27.5K female**.
2. Admissions are balanced: **Elective 18.5K, Urgent 18.4K, Emergency 18.1K**. About one in three admissions is an emergency.
3. Each of the six medical conditions has about **9.1K to 9.2K patients**.
4. **Total revenue is about 1.40bn.** The five insurance providers contribute **277M to 284M** each (Cigna highest, Aetna lowest).
5. **Diabetes earns the most revenue (236M)** and **Cancer the least (230M)**.
6. Average billing is **25K to 26K for every condition**, and the average stay is about **15.5 days**.
7. Monthly admissions range from about **4,200 to 4,800**, with a peak around July-August and a dip in February.
8. The top 10 hospitals earn **0.7M to 1.1M** each, led by Johnson PLC (1.1M).

> The dataset is synthetic, so the findings show the analysis approach, not real hospital performance.

## DAX Measures Used

| Measure | Purpose |
|---|---|
| Total Patients | Count of patient records |
| Total Revenue | Sum of Billing Amount |
| Average Billing | Average Billing Amount per patient |
| Average Length of Stay | Average days between admission and discharge |
| Emergency % | Share of admissions that are Emergency |
| Gender Split | Male and female share of patients |
| Revenue YoY | Revenue change versus the previous year |
| Top Condition | Medical condition with the most patients |
| Selected Hospital | Hospital name shown on the drill-through page |

```DAX
Total Patients = COUNTROWS ( healthcare_dataset )

Total Revenue = SUM ( healthcare_dataset[Billing Amount] )

Average Billing = AVERAGE ( healthcare_dataset[Billing Amount] )

Average Length of Stay =
AVERAGEX (
    healthcare_dataset,
    DATEDIFF ( healthcare_dataset[Date of Admission], healthcare_dataset[Discharge Date], DAY )
)

Emergency % =
DIVIDE (
    CALCULATE ( COUNTROWS ( healthcare_dataset ), healthcare_dataset[Admission Type] = "Emergency" ),
    COUNTROWS ( healthcare_dataset )
)

Male % =
DIVIDE (
    CALCULATE ( COUNTROWS ( healthcare_dataset ), healthcare_dataset[Gender] = "Male" ),
    COUNTROWS ( healthcare_dataset )
)

Revenue YoY % =
VAR CurrentRevenue = [Total Revenue]
VAR PreviousRevenue = CALCULATE ( [Total Revenue], SAMEPERIODLASTYEAR ( calender[Date] ) )
RETURN DIVIDE ( CurrentRevenue - PreviousRevenue, PreviousRevenue )

Top Condition =
MAXX (
    TOPN ( 1, VALUES ( healthcare_dataset[Medical Condition] ), [Total Patients] ),
    healthcare_dataset[Medical Condition]
)

Selected Hospital =
SELECTEDVALUE ( healthcare_dataset[Hospital details], "All Hospitals" )
```

## Recommendations

- Plan staffing and capacity for the July-August peak, and use February for maintenance and training.
- Keep emergency capacity steady, since about 33% of admissions are emergencies.
- Check why Cancer earns the least although its patient volume is similar to other conditions.
- Use the drill-through page to review hospitals over time. Per-hospital volumes are small (about 30-40 patients), so avoid judging on single numbers.

## Repository Structure
Healthcare_Analysis_Project-PowerBi/
-Healthcare_Analysis_Powerbiproject.pbix
-Healthcare_Project_Summary(powerbi) (1).pdf
-Healthcare_powerbi_page(3).jpg.jpeg
-Patient Review.jpg.jpeg
-README.md
-healthcare_dataset.csv
-healthcare_powerbi_page1.jpg.jpeg
## How to Use

1. Download `Healthcare_Dashboard.pbix`.
2. Open it in Power BI Desktop (free).
3. Use the slicers on the left, the page buttons to move around, and right-click a hospital on page 2 to drill through.

## Author

**Boomika**
Data Analyst
