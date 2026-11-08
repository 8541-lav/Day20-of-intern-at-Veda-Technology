HR Attrition Dashboard (Task 20)

Track: Data Analytics | Tools: Power BI, Excel | Dataset: IBM HR Analytics Employee Attrition (WA_Fn-UseC_-HR-Employee-Attrition.csv)

Objective

Turn HR data into management insights by showing attrition by department, job role and tenure, using attrition rate (not raw counts alone).

Dataset snapshot
Item	Value
Employees (rows)	1,470
Columns	35
Missing values	0
Leavers (Attrition = Yes)	237
Overall attrition rate	16.1%

Note: this is a synthetic dataset created by IBM data scientists, so findings illustrate method and are not real company facts.

Deliverables
Dashboard: built in Power BI (steps below). Excel PivotTables are used for validation.
5 insights: see REPORT.md.
Key metric
Attrition Rate = Leavers / Total Employees in the segment

Rates make segments of different sizes comparable. For example, R&D has the most leavers (133) but a lower rate (13.8%) than Sales (20.6%).

Dashboard layout (Power BI)
Area	Visual	Fields
Top row	KPI cards	Total Employees, Leavers, Attrition Rate
Left	Bar chart	Attrition Rate by Department
Centre	Sorted bar chart	Attrition Rate by Job Role (with headcount in tooltip)
Right	Column chart	Attrition Rate by Tenure band
Bottom	Supporting visuals	Overtime, Job Level, Income quartile
Side	Slicers	Department, Overtime, Business Travel
Steps to rebuild
Load: Power BI Desktop > Get Data > Text/CSV > select the dataset > Transform Data.
Clean/protect: remove EmployeeNumber, EmployeeCount, Over18, StandardHours (no analytic value or identifying). Keep only fields needed.
Add columns (DAX)
DAX
   Left = IF('HR'[Attrition] = "Yes", 1, 0)

   Tenure Band =
   SWITCH(TRUE(),
       'HR'[YearsAtCompany] <= 1, "0-1 yrs",
       'HR'[YearsAtCompany] <= 3, "2-3 yrs",
       'HR'[YearsAtCompany] <= 5, "4-5 yrs",
       'HR'[YearsAtCompany] <= 10, "6-10 yrs",
       "11+ yrs")
Measures (DAX)
DAX
   Total Employees = COUNTROWS('HR')
   Leavers = SUM('HR'[Left])
   Attrition Rate = DIVIDE([Leavers], [Total Employees])
Build visuals per the layout table; format Attrition Rate as 0.0%; sort bars descending; add a constant line at the 16.1% company average.
Validate in Excel: Insert PivotTable, rows = Department / JobRole, values = Count of Attrition and Average of Left (should match Power BI).
Publish/export: save .pbix and export a PDF/screenshot.
Protecting sensitive details
Drop ID and constant columns before sharing.
No employee-level tables on the dashboard; aggregate views only.
Suppress or merge segments with very small headcount (e.g. fewer than ~10) to avoid singling out individuals.
Demographics (age, gender, marital status) are not used to target people; they are shown only as context and for fairness monitoring.
Share attrition insights by group, never by named employee.
Interview prep

Why use attrition rate? Counts depend on segment size; rates show risk. A big team can have many leavers yet low risk, and a tiny team can have few leavers yet very high risk (Sales Representative: 33 leavers, 39.8% rate).

What segments matter? Those that are both actionable and high-risk: job role, department, overtime, job level, tenure band, pay quartile, and business travel.

Files
README.md   this file
REPORT.md   findings and recommendations
