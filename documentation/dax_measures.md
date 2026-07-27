# DAX Measures

## Overview

This dashboard uses DAX to calculate dynamic statistics and enable interactive analysis of Formula 1 drivers, constructors, races, and championships.

The most technically challenging feature was implementing an independent driver comparison tool that allows two drivers to be selected simultaneously without filter conflicts.

---

# Driver Comparison

## Problem

Initially, both comparison slicers were connected to the same `drivers` table.

This caused Power BI to share the same filter context between both slicers. Selecting a driver in the first slicer filtered the second slicer, making independent comparisons impossible.

---

## Solution

To solve this issue, two disconnected tables were created:

```DAX
Driver1Choice = DISTINCT(drivers[surname])

Driver2Choice = DISTINCT(drivers[surname])
```

These tables have **no relationships** with the rest of the data model.

Instead, DAX measures propagate the selected driver into the model using **virtual relationships** created with `TREATAS()`.

This allows each comparison panel to calculate statistics independently.

---
# Dynamic Interface Design

## Project Constraint

One of the project requirements was that **all functionality had to fit within a single report page**.

Rather than creating separate report pages for Drivers, Constructors, Circuits, and Comparisons, the dashboard had to provide all views from a single canvas.

This introduced several UI and interaction challenges.

---

## Solution

To maximize the available space while maintaining usability, the dashboard was designed as an interactive interface rather than a traditional static report.

The implementation included:

- Navigation buttons
- Bookmarks
- Selection panes
- Dynamic object visibility
- Layered visuals
- Conditional titles and labels
- DAX-driven content updates

Instead of navigating between report pages, users interact with buttons that reveal or hide groups of visuals.

This creates the experience of multiple dashboards while remaining within a single Power BI page.

---

## Benefits

This design:

- Reduced navigation complexity
- Preserved screen space
- Improved dashboard usability
- Allowed richer interaction despite the one-page limitation

---

# Example Measure

```DAX
D1 Wins =
IF(
    HASONEVALUE(Driver1Choice[surname]),
    CALCULATE(
        COUNTROWS(results),
        results[positionOrder] = 1,
        TREATAS(
            VALUES(Driver1Choice[surname]),
            drivers[surname]
        )
    ),
    BLANK()
)
```

This measure:

- Ensures a single driver is selected.
- Creates a virtual relationship between the disconnected slicer and the `drivers` table.
- Counts all race victories for the selected driver.
- Returns a blank value if no driver has been selected.

---

# Removing Shared Filter Context

The second comparison panel uses `REMOVEFILTERS()` to prevent the first driver's selection from affecting the second.

Example:

```DAX
D2 Points =
CALCULATE(
    SUM(results[points]),
    REMOVEFILTERS(drivers),
    TREATAS(
        VALUES(Driver2Choice[surname]),
        drivers[surname]
    )
)
```

This guarantees that Driver 2 statistics are calculated independently.

---

# Other Measures

Additional DAX measures calculate:

- Career Wins
- Total Podiums
- Championship Points
- Fastest Lap Time
- Maximum Recorded Speed
- Constructor
- Driver Number
- Nationality
- Driver Code
- Driver Identifier

These measures update dynamically based on the currently selected driver.

---

# DAX Functions Used

| Function | Purpose |
|-----------|---------|
| CALCULATE() | Changes filter context for calculations |
| TREATAS() | Creates virtual relationships between disconnected tables |
| REMOVEFILTERS() | Removes existing filters before calculation |
| HASONEVALUE() | Verifies that exactly one value has been selected |
| SELECTEDVALUE() | Returns the currently selected value |
| VALUES() | Retrieves selected values from slicers |
| COUNTROWS() | Counts wins and podium finishes |
| SUM() | Calculates cumulative statistics |
| MIN() | Retrieves fastest lap time |
| MAX() | Retrieves maximum speed |
| TOPN() | Returns the latest constructor information |

---

# Skills Demonstrated

- Advanced DAX
- Filter Context Management
- Virtual Relationships
- Interactive Dashboard Design
- Dynamic KPI Development
- Performance-Oriented Data Modeling