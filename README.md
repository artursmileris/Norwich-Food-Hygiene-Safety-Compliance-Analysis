# Norwich Food Hygiene & Safety Compliance Analysis

Data preparation and analysis project that transforms official Food Hygiene Rating Scheme (FHRS) XML data for Norwich into a clean, analysis-ready Excel Table using Power Query, then explores compliance patterns with advanced Pivot Tables.

---

## Project Overview

This project demonstrates a complete workflow covering:

- **Data extraction** – loading the official FHRS XML extract for Norwich City Council
- **Data cleaning & transformation** – expanding nested XML structures, setting data types, and creating derived columns entirely in Power Query
- **Feature engineering** – building helper columns for rating categories, compliance flags, postcode districts, and inspection years
- **Analysis** – creating multiple Pivot Tables to explore rating distributions, compliance rates, average inspection scores, geographic patterns, and trends over time
- **Insight generation** – identifying which business types and areas perform best/worst on food hygiene standards

The goal is to showcase core Level 3 Data Technician skills: Power Query data preparation, Excel Tables, Pivot Tables, and clear documentation of a reproducible analysis process.

**Data source:** Food Hygiene Rating Scheme (FHRS) – Norwich City Council  
**Extract date:** 21 July 2026  
**Records:** 1,563 food establishments  

---

## Key Results (from Pivot Table analysis)

### Overall Compliance Snapshot

| Metric                              | Value      |
|-------------------------------------|------------|
| Total establishments                | 1,563      |
| Rated establishments                | ~1,355     |
| Rated 5 (Very Good)                 | 890        |
| Rated 4 or 5 (Compliant)            | 1,218      |
| Overall compliance rate             | **77.9%**  |
| Awaiting Inspection                 | 172        |
| Rated 0–2 (Improvement needed)      | 23         |

### Business Type Highlights
- Largest category: Restaurant/Cafe/Canteen (447 premises)
- Strong compliance in many retail and care settings
- Takeaways, pubs and mobile caterers show more variation in scores
- No premises in the dataset currently hold a rating of 0 (Urgent Improvement Necessary)

### Geographic Notes
- Main postcode districts: NR2 (448), NR1 (360), NR3 (321)
- Useful for comparing city-centre vs outer-area performance

---

## Step-by-Step Pipeline

### Step 1 – Source Data
- Official FHRS XML file (`FHRS232en-GB.xml`)
- Structure: Header + EstablishmentCollection of EstablishmentDetail records
- Nested elements: Scores (Hygiene, Structural, ConfidenceInManagement) and Geocode (Longitude, Latitude)

### Step 2 – Connect & Expand in Power Query
- Data → Get Data → From File → From XML
- Expand `Scores` into Score_Hygiene, Score_Structural, Score_ConfidenceInManagement
- Expand `Geocode` into Longitude and Latitude
- Handle `xsi:nil` attributes on RatingDate by expanding Element:Text where required

### Step 3 – Data Types
- Whole Number: FHRSID, BusinessTypeID, scores (after conversion), TotalScore
- Date: RatingDate
- Decimal Number: Longitude, Latitude
- True/False (later converted to 1/0 for reliable Pivot aggregation): IsRated, IsCompliant, IsExcellent
- Text: all remaining descriptive fields

### Step 4 – Derived Columns (Power Query Custom / Conditional Columns)
| Column              | Purpose / Logic                                      |
|---------------------|------------------------------------------------------|
| TotalScore          | Sum of the three component scores                    |
| InspectionYear      | Year extracted from RatingDate                       |
| InspectionMonth     | yyyy-MM from RatingDate                              |
| PostcodeDistrict    | Outward code via Text.BeforeDelimiter                |
| RatingCategory      | Friendly labels (5 - Very Good, 4 - Good, etc.)      |
| RatingNumeric       | Numeric conversion of RatingValue (null if unrated)  |
| IsRated             | TRUE if rating is 0–5                                |
| IsCompliant         | TRUE if rating is 4 or 5                             |
| IsExcellent         | TRUE if rating is exactly 5                          |

### Step 5 – Clean & Shape
- Remove low-value columns (LocalAuthorityCode, websites, emails, RatingKey, RightToReply, LocalAuthorityBusinessID, etc.)
- Reorder columns into a logical analysis-friendly sequence
- Leave nulls in place for unscored / awaiting-inspection premises (correct behaviour for averages)

### Step 6 – Load
- Close & Load To → Table on a new worksheet
- Name the Excel Table `FoodHygieneData`

### Step 7 – Pivot Table Analysis
Eight Pivot Tables (and supporting calculations) were built:

1. Rating distribution by Business Type  
2. Compliance rate by Business Type  
3. Average scores by Business Type  
4. Performance by Postcode District  
5. Inspection trends over time  
6. Score components by final rating  
7. Awaiting Inspection / Exempt breakdown  
8. High / low performers (filtered views)

Slicers can be added on BusinessType and PostcodeDistrict for interactivity.

---

## Important Notes

1. **FHRS scoring direction** – Lower component scores (Hygiene, Structural, ConfidenceInManagement) are better. A score of 0 is the best possible result on each measure.
2. **Missing scores** – Premises marked Awaiting Inspection or Exempt correctly have blank scores and dates. These are left as null so Pivot Table averages ignore them.
3. **Boolean aggregation** – True/False flags were converted to 1/0 in Power Query for reliable Sum / Average behaviour inside Pivot Tables.
4. **No rating of 0** – The Norwich extract contains no establishments with the worst possible rating at the time of extract.
5. **Refreshability** – The entire cleaning process is stored in Power Query Applied Steps and can be refreshed if a newer XML extract is supplied.

---

## Tech Stack

| Category            | Tools                                      |
|---------------------|--------------------------------------------|
| Data Source         | FHRS open data (XML)                       |
| Data Preparation    | Power Query (Get & Transform)              |
| Analysis            | Excel Pivot Tables, Pivot Charts, Slicers  |
| Documentation       | Markdown                                   |
| Environment         | Microsoft Excel                            |

---

## Repository Structure

```
norwich-food-hygiene-analysis/
├── README.md
├── data/
│   └── FHRS232en-GB.xml          # Raw source file
├── Norwich_Food_Hygiene_Analysis.xlsx   # Cleaned data + Pivot Tables
└── docs/                         # Optional supporting notes
```

---

## Getting Started

### Prerequisites
- Microsoft Excel (Microsoft 365 or Excel 2016+ recommended for full Power Query support)

### Steps to reproduce
1. Place the raw `FHRS232en-GB.xml` file in an accessible location.
2. Open a new Excel workbook.
3. Follow the Power Query steps documented in this README (or the Applied Steps already present in the accompanying workbook).
4. Load the result as an Excel Table named `FoodHygieneData`.
5. Build the Pivot Tables listed in Step 7.

### Refreshing the data
If a newer FHRS extract becomes available:
- Data → Queries & Connections → right-click the query → Refresh
- All downstream Pivot Tables will update after refresh.

---

## Skills Demonstrated

- Connecting to and shaping hierarchical XML data in Power Query
- Expanding nested records and handling nil attributes
- Creating conditional and custom columns
- Data type management and null handling
- Building analysis-ready Excel Tables
- Designing multiple Pivot Tables for different analytical questions
- Calculating compliance rates and interpreting FHRS scores
- Clear, reproducible documentation of a data workflow

---

## Future Improvements

- Add Slicers and a simple dashboard sheet
- Compare Norwich results with another local authority
- Create Pivot Charts for the main distributions
- Add a short written findings summary for stakeholders
- Parameterise the Power Query source path for easier reuse
