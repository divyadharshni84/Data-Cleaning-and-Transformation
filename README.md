# Data-Cleaning-and-Transformation

# Project Overview
This project transforms raw student data into an interactive decision-making tool. It focuses on identifying high-value courses, student demographics (age-group analysis), and seasonal enrollment trends to help educational administrators optimize course offerings and marketing spend.

# Core Technical Workflow
1. Data Cleaning & Transformation (Power Query)
Since no external scripts were used, the heavy lifting was performed in Power Query Editor to ensure a "Single Version of Truth":

Data Profiling: Removed null values and duplicates from student records.

Type Casting: Corrected data types for currency (Fees), dates (Date of Join), and geographical data (City, State).

Custom Columns: Created conditional logic to handle inconsistencies in course naming conventions.

Attribute Grouping: Developed the Age (groups) binning logic to segment students into the four primary buckets seen in the donut chart (Below 20, 20-29, 30-39, Above 40).

2. Data Modeling
The project uses a structured relationship model between two primary tables:

Personal Details: Contains unique student attributes (ID, Contact, Location).

Student Course Details: Contains transactional data (Fees, Course Name, Enrollment Date).

Relationship: Linked via a 1-to-many relationship on Student Name / ID to allow for cross-filtering across all visuals

# DAX Measures & Logic
All calculations were performed natively within Power BI using DAX to drive the KPIs, trends, and demographic segments.

1. Key Performance Indicators (KPIs)
These measures power the high-level summary cards at the top of the dashboard.

Code snippet
// Total revenue generated across all courses
Total Amount = SUM('Student Course Details'[Fees])

// Distinct count of students to avoid double-counting multi-course enrollees
Total Students = DISTINCTCOUNT('Personal Details'[Student Name])
2. Certification Tracking
This logic drives the Gauge visual, comparing students who received certificates against the total student body.

Code snippet
// Counts only students where 'Issued Certificate' is marked "Yes"
Certificates Issued = 
CALCULATE(
    COUNT('Personal Details'[Issued Certificate]),
    'Personal Details'[Issued Certificate] = "Yes"
)
3. Demographic Segmentation
To create the donut chart, a Calculated Column was used to bucket raw ages into meaningful professional groups.

Code snippet
// Categorizing students into age brackets for demographic analysis
Age Group = 
SWITCH(
    TRUE(),
    'Student Course Details'[Age] < 20, "Below 20",
    'Student Course Details'[Age] <= 29, "20 to 29",
    'Student Course Details'[Age] <= 39, "30 to 39",
    "Above 40"
)
4. Enrollment Trends
The "Count of Course by Month" utilizes the native Date Hierarchy, but the underlying implicit measure is:

Code snippet
// Total volume of course enrollments
Course Count = COUNT('Student Course Details'[Course])

# Tools Used
Power BI Desktop: Entire ETL, Modeling, and Visualization.

Power Query: Data cleaning and schema transformation.

DAX: Advanced measures for KPIs and gauge tracking.

# Dashboard 
![Dashboard Overview](./FOLDER_NAME/Student Enrollment & Revenue Analytics Dashboard.png)

