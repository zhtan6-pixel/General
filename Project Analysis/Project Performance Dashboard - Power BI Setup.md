# Project Performance Dashboard

This folder contains the project assets for building the Power BI dashboard from [Dataset.csv](Dataset.csv).

## Recommended Power BI data model

### Fact table
- Use Dataset.csv as the fact table
- Keep one row per WBS element
- Aggregate to project level in visuals when needed

### Dimensions to create
- Project dimension
- Date table
- Department dimension
- Region dimension
- Project Type dimension
- Manager dimension
- Risk dimension
- Status dimension

## Recommended measures

```DAX
Total Budget = SUM(ProjectData[Budget_USD])
Actual Cost = SUM(ProjectData[Actual_Cost_USD])
Committed Cost = SUM(ProjectData[Committed_Cost_USD])
Forecast Cost = SUM(ProjectData[Forecast_Cost_USD])
Actual Revenue = SUM(ProjectData[Actual_Revenue_USD])
Planned Revenue = SUM(ProjectData[Planned_Revenue_USD])
Project Margin = [Actual Revenue] - [Actual Cost]
Margin % = DIVIDE([Project Margin], [Actual Revenue])
Cost Variance = [Forecast Cost] - [Actual Cost]
Cost Variance % = DIVIDE([Cost Variance], [Forecast Cost])
Avg CPI = AVERAGE(ProjectData[Cost_Performance_Index])
Avg SPI = AVERAGE(ProjectData[Schedule_Performance_Index])
Avg Risk Score = AVERAGE(ProjectData[Risk_Score])
Milestone Completion % = AVERAGE(ProjectData[Milestone_Completion_Pct])
Schedule Variance Days = SUM(ProjectData[Schedule_Variance_Days])
Actual Hours = SUM(ProjectData[Actual_Hours])
Planned Hours = SUM(ProjectData[Planned_Hours])
Hours Variance = [Planned Hours] - [Actual Hours]
```

## Project dimension

```DAX
Project Dim =
SUMMARIZE(
    ProjectData,
    ProjectData[Project_ID],
    ProjectData[Project_Name],
    ProjectData[Project_Type],
    ProjectData[Department],
    ProjectData[Region],
    ProjectData[Project_Manager],
    ProjectData[Project_Start_Date],
    ProjectData[Project_End_Date]
)
```

## Date table

```DAX
DateTable =
ADDCOLUMNS(
    CALENDAR(MIN(ProjectData[Project_Start_Date]), MAX(ProjectData[Project_End_Date])),
    "Year", YEAR([Date]),
    "Month", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM")
)
```

## Recommended dashboard pages

### Page 1: Executive Overview
- KPI cards: Total Budget, Actual Cost, Forecast Cost, Actual Revenue, Project Margin, Avg CPI
- Bar chart: Project Name by Actual Cost
- Stacked bar: Region by Budget vs Actual Cost
- Matrix: Department / Project / Budget / Actual Cost / Margin
- Slicers: Department, Region, Project Type, Project Manager

### Page 2: Schedule & Delivery
- KPI: Avg SPI
- KPI: Milestone Completion %
- Column chart: Project by Schedule Variance Days
- Matrix: Project vs Planned Hours / Actual Hours / Hours Variance
- Slicer: SAP_System_Status

### Page 3: Risk & Health
- Matrix: Project / Avg Risk Score / Avg CPI / Avg SPI / Status
- Scatter chart: Avg CPI vs Avg SPI
- Table: Project, Risk Category, Risk Score, Milestone Completion %, Schedule Variance

## Suggested title
Project Performance Dashboard

## Suggested theme colors
- Dark navy: #0F172A
- Blue accent: #2563EB
- Green: #10B981
- Amber: #F59E0B
- Red: #EF4444
- Light gray: #E5E7EB

## Next step
Open this folder in Power BI Desktop, import Dataset.csv, create the measures above, and build the visuals.

A true .pbix file must be created inside Power BI Desktop itself; this folder contains the ready-to-use design and DAX logic to build the report.
