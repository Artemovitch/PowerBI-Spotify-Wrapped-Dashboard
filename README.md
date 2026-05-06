# Personal Spotify Listening Analysis

## Project Overview
This project is an end-to-end data pipeline and visualization of my personal Spotify listening history. The goal was to analyze my listening trends, track frequencies, and artist preferences over time. By applying dimensional modeling best practices, I transformed raw JSON data into an interactive Power BI dashboard capable of dynamic time-intelligence reporting and behavioral analysis.

**Project Workflow:**
* **Data Extraction:** Requested and retrieved extended listening history from Spotify as multiple raw JSON files.
* **Data Transformation:** Utilized Python to aggregate, explore, and clean the data into a single, analysis-ready dataset.
* **Data Modeling & Visualization:** Imported the dataset into Power BI to construct the dimensional model, write custom DAX measures, and build the interactive UI.

**Tools Used:** Python (Data Cleaning/ETL), Power BI (Data Modeling, DAX, Visualization)

## Visuals
1. **Main Dashboard:** High-level KPIs, Year-over-Year (YoY) variance, and Top 5 breakdowns for Albums, Artists, and Tracks.
<img width="1576" height="1051" alt="image" src="https://github.com/user-attachments/assets/ade108ad-00e8-4c62-b417-60cd7edf60ac" />

2. **Listening Patterns:** A matrix heatmap showing listening intensity by hour and day of the week, alongside a dynamic scatter plot analyzing average listening time versus track frequency using what-if parameters.
<img width="1574" height="1051" alt="image" src="https://github.com/user-attachments/assets/9f72f8a3-de63-4a73-af37-db8f57b6674b" />
<img width="1577" height="1052" alt="image" src="https://github.com/user-attachments/assets/94355bba-42b3-4047-bb61-1c43ba47d654" />

3. **Data Model:** A star schema architecture featuring a centralized fact table connected to a custom Date table and disconnected parameter tables.
<img width="861" height="612" alt="image" src="https://github.com/user-attachments/assets/31c2331f-28f9-46d7-a970-ac160dc15cbe" />

## Data Architecture & Modeling
* **ETL Pipeline:** Extracted raw JSON files from Spotify. Used Python to flatten nested arrays, handle null values, extract `minutes_played` and `seconds_played` from `ms_played`, and strip out unused columns to optimize file size.
* **Dimensional Modeling:** Built a star schema in Power BI. 
* **Time Intelligence:** Created a dedicated `Date Table` to enable time-based DAX calculations (YoY variance, Weekday vs. Weekend segmentation).
* **Dynamic Parameters:** Implemented disconnected parameter tables (`Track Frequency` and `Listening Time`) to allow users to dynamically adjust the axes and thresholds on the scatter plot visual.

## Key DAX Measures
To power the dynamic KPI cards on the main dashboard, I wrote custom DAX measures that handle both the mathematical variance calculations and the front-end string formatting. Here is the logic used for the Tracks Played KPI:

```dax
PY and YoY Tracks KPI = 

VAR _latest = [LatestYearTracks]
VAR _previous = [PreviousYearTracks]

VAR _YoY = IF(NOT(ISBLANK(_previous)), DIVIDE(_latest - _previous, _previous, 0), BLANK())

RETURN

IF(NOT(ISBLANK(_previous)), "vs PY: " & FORMAT(_previous, "#,##0") & " (" & FORMAT(_YoY, "0.00%") & ")", "No Data")
```

**Project Insights & Future Enhancements**
Building this dimensional model revealed a few interesting behavioral trends, such as a heavy reliance on my top 5 artists despite exploring nearly 6,000 unique creators across the dataset.

For future iterations of this project, I plan to:

* **Incorporate an API:** Automate the data pipeline by connecting directly to the Spotify Web API instead of relying on static JSON exports.

* **Genre Analysis:** Integrate external metadata to categorize artists by genre and track shifting musical tastes over time.
