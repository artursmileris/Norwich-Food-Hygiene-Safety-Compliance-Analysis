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

### Step 1 – Obtain & Import the Data
- Downloaded the official FHRS XML extract for Norwich City (`FHRS232en-GB.xml`) from the Food Standards Agency open data
- Imported the file into Excel using **Data → Get Data → From File → From XML**
- Expanded the nested `Scores` and `Geocode` records and loaded the data into Power Query

### Step 2 – Clean & Transform in Power Query
- Expanded nested columns and set correct data types
- Created derived columns: `TotalScore`, `InspectionYear`, `InspectionMonth`, `PostcodeDistrict`, `RatingCategory`, `RatingNumeric`, `IsRated`, `IsCompliant`, `IsExcellent`
- Converted compliance flags to 1/0 for reliable Pivot Table aggregation
- Removed unnecessary columns and reordered the remaining fields
- Loaded the result as an Excel Table named `FoodHygieneData`

### Step 3 – Pivot Table Analysis
Eight Pivot Tables were created to answer key questions:

1. Rating distribution by Business Type  
2. Compliance rate by Business Type  
3. Average scores by Business Type  
4. Performance by Postcode District  
5. Inspection trends over time  
6. Score components by final rating  
7. Awaiting Inspection / Exempt breakdown  
8. High / low performers (filtered views)

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
└── Norwich_Food_Hygiene_Analysis.xlsx   # Cleaned data + Pivot Tables
```

---

## Skills Demonstrated

- Connecting to and shaping hierarchical XML data in Power Query
- Expanding nested records and handling nil attributes
- Creating conditional and custom columns
- Data type management and null handling
- Building analysis-ready Excel Tables
- Designing multiple Pivot Tables for different analytical questions
- Calculating compliance rates and interpreting FHRS scores
- Clear documentation of a data workflow

---

## Future Improvements

- Add Slicers and a simple dashboard sheet
- Compare Norwich results with another local authority
- Create Pivot Charts for the main distributions
- Add a short written findings summary for stakeholders
- Parameterise the Power Query source path for easier reuse
