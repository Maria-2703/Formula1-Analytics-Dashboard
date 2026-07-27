# Data Model

## Overview

The dashboard is built using a relational data model based on historical Formula 1 datasets.

The objective was to create a scalable model that supports interactive filtering while minimizing redundancy and maintaining good query performance.

---

# Tables

The model integrates multiple datasets, including:

- Drivers
- Constructors
- Results
- Races
- Circuits
- Seasons
- Driver Standings
- Constructor Standings
- Qualifying Results
- Sprint Results (where available)

---

# Relationships

The model follows a relational design where fact tables reference shared dimension tables.

Example:

```
Drivers
      │
      │
Results ───── Races ───── Circuits
      │
Constructors
```

This structure allows users to filter reports by:

- Driver
- Constructor
- Circuit
- Season
- Country
- Nationality

without duplicating data.

---

# Data Preparation

Data preparation was performed using Power Query.

The workflow included:

- Importing multiple CSV datasets
- Correcting data types
- Removing inconsistencies
- Handling missing values
- Standardizing column names
- Creating calculated columns where appropriate

---

# Data Modeling Decisions

Several modeling decisions were made to improve usability and maintainability.

# Dashboard Architecture

Unlike traditional Power BI reports composed of multiple pages, this dashboard was intentionally designed as a **single-page application**.

The report uses:

- Bookmarks
- Selection Pane
- Dynamic visual visibility
- Interactive navigation buttons

Groups of visuals are layered on top of each other and displayed only when relevant.

This approach minimizes navigation while allowing users to explore multiple analytical perspectives from one report page.

---

# Disconnected Tables

Two additional tables were created:

- Driver1Choice
- Driver2Choice

These tables are intentionally disconnected from the model.

They support independent driver comparison using DAX virtual relationships (`TREATAS()`).

---

# Performance Considerations

To improve report responsiveness:

- Relationships were kept as simple as possible.
- Duplicate information was avoided.
- Measures were preferred over calculated columns whenever appropriate.
- Filter context was controlled using DAX rather than unnecessary relationships.

---

# Technologies Used

- Power BI Desktop
- Power Query (M)
- DAX
- CSV Data Sources
