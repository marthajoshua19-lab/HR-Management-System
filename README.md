# HR-Attrition-Analysis

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning/preparation](#data-cleaning/prepartion)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#results/findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [References](#references)
  
### Project Overview

This project analyses employee turnover to understand why employees leave and identify ways organisations can improve staff retention.

<img width="606" height="341" alt="Dashboard 1" src="https://github.com/user-attachments/assets/65def364-e9ed-442f-92a9-efda381e65ba" />



### Data Source

The primary dataset used is the 'IBM HR Analytics Employee Attrition & Performance.CSV File' containing information about each employee attrition and performance in the Company.

### Tools

- Excel - Data Cleaning [Download here](https://microsoft.com)
- SQL Server - Data Analysis [Download here](https://microsofft.com)
- PowerBI - Creating Dashboard reports [Download here](https://microsoft.com)

### Data Cleaning/Preparation

In the initial data prepartion phase, we performed the following task:
1. Data loading and thorough inspection.
2. Handlin missing/duplicate values.
3. Data cleaning and formatting

### Exploratory Data Analysis

EDA involved exploring the HR Attrtion data to answer key questions, such as:

1. What is the overall attrition rate? 
2. What is the attrition rate per department and per job role? 
3. Do employees who work overtime leave more often? 
4. How does attrition vary by age group and by tenure band? 
5. What is the average monthly income of employees who left vs stayed? 
6. How does attrition change with job satisfaction and work-life balance scores? 
7. Does distance from home or business travel frequency relate to attrition? 
8. Which 5 job roles have the longest average time since last promotion?

### Data Analysis

Include code/features worked with

```SQL
use Group_DDgoSelect Count(*) as TotalEmployees, Sum(Attrition_Flag) as Employees_Who_Left, Cast(Sum(Attrition_Flag) * 100.0 / count (*) as Decimal(10,2))As Overall_Attrition_Rate from Attrition_Project
```

```SQL
use Group_DDgoselect Department, Job_Role, count(*) As Total_Employees,sum(Attrition_Flag)
 As Employees_Who_Left, Cast(sum(Attrition_Flag) * 100.0/ count (*) As Decimal(10,2)) As Attrition_Rate from Attrition_Project
Group by Department, Job_Role
```

```SQL
use Group_DD go select Over_Time,count(*) As Total_Employee,sum(Attrition_Flag)As Employees_Who_Left, Cast (sum(Attrition_Flag) * 100.0/ count (*) As Decimal (10,2))As Attrition_Rate from Attrition_Project
Group by Over_Time
```

```SQL
use Group_DD
go
select Age_Band, Tenure_Band,
Total_Employees, Employees_Who_Left, (Employees_Who_Left * 100.0 / Total_Employees) 
As Attrition_Rate 
From
( select Age_Band, Tenure_Band, Count(*) As Total_Employees, 
sum(case when Attrition_Flag = 1 then 1 else 0 end)
As Employees_Who_Left from Attrition_Project
Group by Age_Band, Tenure_Band)
As AgeTenureSummary
Order by Age_Band, Tenure_Band
```

```SQL
use Group_DD
  go
  select (Attrition_Flag)
  As Employees_Status, 
  Average_Monthly_Income
  from
  (select Attrition_Flag, Avg(Sum_of_MonthlyIncome) As Average_Monthly_Income from Attrition_Project
  Group by Attrition_Flag) 
  as Income_Summary
```

```SQL
 use Group_DD
  go
  Select Sum_of_JobSatisfaction, Work_Life_Balance, Attrition_Flag, count(*) As Total_Employees,
Sum(case when Attrition_Flag = 1 then 1  else 0 end)
  As Employees_Who_Left, 
  Sum(case when Attrition_Flag = 1 then 1  else 0 end) * 100.0/Count (*) As Attrition_Rate 
  from Attrition_Project
  Group by Sum_of_JobSatisfaction, Work_Life_Balance, Attrition_Flag
```

```SQL
use Group_DD
go
Select 
'Sum_of_DistanceFromHome' As Factor,
Cast(Sum_of_DistanceFromHome as Varchar(50)) As Factor_Value,
Count(*) as Total_Employees,
Sum(Attrition_Flag) as Employees_Who_Left, 
sum(Attrition_Flag) * 100.0 /count (*) As Attrition_Rate from Atttrition_Project
Group by Sum_of_DistanceFromHome

Union All

Select 
'Business_Travel' As Factor,
Business_Travel As Factor_Value, 
Count(*) as Total_Employees,
Sum(Attrition_Flag) as Employees_Who_Left, 
sum(Attrition_Flag) * 100.0 / count (*) As Attrition_Rate from Attrition_Project
Group by Business_Travel
```

```SQL
use Group_DD
 go
select Top 5 JobRole,
Avg(Sum_of_YearsSinceLastPromotion) As Average_Years
From Attrition_Project
Group by JobRole
Order by Average_Years Desc
```

### Results/Findings

The analysis results are summarized as follows:
1. The analysis covered 1,470 employees. A total of 237 employees left the organisation, representing an attrition rate of 16.12%. 1,233 employees remained with the organisation.
2. The analysis compared employee exits across the different departments to identify where turnover was highest.
3. The analysis identified differences in employee exits across job roles
4. The analysis compared employees who worked overtime with those who did not to identify differences in attrition.
5. Age Group and Attrition: The analysis compared employee exits across different age groups.
6. The analysis examined how employee satisfaction levels related to staff exits.

### Recommendations

Reduce excessive overtime, as employees working overtime have a higher attrition rate.
Improve retention in the Sales and Human Resources departments.
Provide better support and career opportunities for employees below 35 years.
Improve working conditions and engagement for employees with low job satisfaction.
Review salaries and benefits, especially for employees in the low-income group.
Introduce targeted retention plans for Sales Representatives and Laboratory Technicians.
Conduct regular employee satisfaction surveys to identify reasons for staff exits.

### Limitations

The data did not provide detailed reasons why employees left the organisation, making it difficult to determine the exact causes of attrition. Also, the analysis was based only on the information provided, so other factors that may influence employee turnover might not have been considered. The findings show relationships between employee factors and attrition but do not establish that one factor directly causes employees to leave. Finally, the results may not apply to all organisations, as employee experiences and working conditions differ.

### References
1. SQL for beginer by Mr Micheal
2. Report writing Handbook by Mr Micheal
3. [How to upload Projects on GitHub](https://www.youtube.com/watch?v=0N9xekdKCwk)
