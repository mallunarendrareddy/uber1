# NCR Ride Bookings Dashboard — README

## Project Overview

This Power BI project analyzes ride-booking activity in the NCR (National Capital Region) using the `ncr_ride_bookings.csv` dataset. The dataset contains **150,000 records and 21 columns**, including booking dates and times, booking status, vehicle type, pickup/drop locations, trip timing, cancellations, ride value, distance, ratings, and payment method.

The report is organized into these pages:

- **Home Page** — navigation landing page for the report.
- **Overall** — high-level booking overview, including a trend chart, booking-status breakdown, and a date slicer.
- **Vechical Type** — vehicle-type-focused KPIs and comparisons. *(The page name is preserved as it appears in the PBIX.)*
- **Renvenue** — revenue-focused charts and a detail table. *(The page name is preserved as it appears in the PBIX.)*
- **Cancellation** — cancellation analysis, including reason/status breakdowns and KPI cards.
- **Ratings** — rating-related KPI cards and analysis.

The report includes navigation buttons, slicers, cards, charts, and a table. Use the slicers to filter the report and compare the results across dates or categories.

## Dataset

The source file is `ncr_ride_bookings.csv`.

### Main fields

| Field | Description |
|---|---|
| Date, Time | Date and time of the booking |
| Booking ID, Customer ID | Booking and customer identifiers |
| Booking Status | Outcome/status of the booking |
| Vehicle Type | Vehicle category selected |
| Pickup Location, Drop Location | Trip origin and destination |
| Avg VTAT, Avg CTAT | Average vehicle and customer trip/arrival time measures supplied in the data |
| Cancelled Rides by Customer / Driver | Cancellation indicators/counts |
| Customer / Driver cancellation reason | Reasons recorded for cancellations |
| Incomplete Rides / Incomplete Rides Reason | Incomplete-ride indicator and reason |
| Booking Value | Recorded booking value |
| Ride Distance | Recorded trip distance |
| Driver Ratings, Customer Rating | Ratings recorded for the ride |
| Payment Method | Payment type |

## Workflow

### 1. Import the data

1. Open Power BI Desktop.
2. Select **Home → Get data → Text/CSV**.
3. Choose `ncr_ride_bookings.csv`.
4. Select **Transform Data** to open Power Query Editor rather than loading the raw data directly.

### 2. Clean and prepare the data in Power Query Editor

The dataset was prepared in Power Query before building visuals. Typical cleaning steps for this source include:

1. **Promote/check headers** so each column has the correct field name.
2. **Set data types**:
   - `Date` → Date
   - `Time` → Time
   - IDs, statuses, locations, reasons, vehicle type, and payment method → Text
   - Timing, booking value, distance, and ratings → appropriate numeric types
3. **Clean text fields** by trimming extra spaces and standardizing text where needed.
4. **Check identifiers** such as Booking ID and Customer ID for unwanted quotation marks or inconsistent formatting.
5. **Review missing values**. Many fields are naturally blank when they do not apply—for example, cancellation reasons on completed rides or ratings on unsuccessful rides. Keep these blanks where they represent “not applicable”; do not automatically replace them with zero.
6. **Check duplicates and invalid values**. Verify booking IDs and inspect unexpected negative values, out-of-range ratings, or impossible distances/times before removing or correcting anything.
7. **Apply and load** using **Close & Apply**.

> Document the exact transformations you performed in Power Query here if you want this README to serve as a reproducible audit log. The steps above describe the preparation workflow, not a claim that every optional check was applied.

### 3. Create measures in Data Modeling

After loading the cleaned table, measures were created in **Modeling → New measure** (or **Data view → New measure**) using DAX. Measures calculate dynamically in response to report filters and slicers.

Examples of common measures for this dataset are shown below. These are illustrative; retain or replace them with the exact measures in your PBIX.

```DAX
Total Bookings =
COUNTROWS('ncr_ride_bookings')

Completed Rides =
CALCULATE(
    [Total Bookings],
    'ncr_ride_bookings'[Booking Status] = "Completed"
)

Total Booking Value =
SUM('ncr_ride_bookings'[Booking Value])

Completion Rate =
DIVIDE([Completed Rides], [Total Bookings], 0)

Average Ride Distance =
AVERAGE('ncr_ride_bookings'[Ride Distance])

Average Driver Rating =
AVERAGE('ncr_ride_bookings'[Driver Ratings])

Average Customer Rating =
AVERAGE('ncr_ride_bookings'[Customer Rating])
```

**Important:** If your Power BI table has a different name, replace `'ncr_ride_bookings'` with the actual table name. The sample measures are not a verified inventory of the measures in the PBIX.

### 4. Build the report visuals

1. Add KPI **Card** visuals for important totals, averages, and rates.
2. Add charts to compare booking status, vehicle type, revenue, cancellation reasons, and ratings.
3. Add a **Date slicer** and any category slicers used in the report.
4. Add a **Table** visual where detailed revenue or booking-level information is useful.
5. Add navigation buttons to move between report pages.
6. Format titles, labels, number formats, colors, and layout consistently.
7. Test interactions: change slicers, select chart segments, and confirm the cards and other visuals respond as expected.

## Dashboard Page Guide

### Overall
Provides a summary of booking activity. The trend chart helps inspect changes over time, the status chart shows the distribution of booking outcomes, and the date slicer narrows the reporting period.

### Vechical Type
Focuses on comparisons among vehicle categories. KPI cards support quick comparison of activity and performance by vehicle type; use the page slicer to focus on a category.

### Renvenue
Focuses on booking value/revenue patterns. Charts compare revenue across categories or time, while the table provides more detailed values. Use the available slicer to filter the analysis.

### Cancellation
Examines cancellation activity and its recorded reasons. The breakdown charts help identify common cancellation patterns, while KPI cards summarize cancellation-related measures.

### Ratings
Focuses on driver and customer ratings. KPI cards provide rating summaries and support comparison of service experience.

## How to Use the Dashboard

1. Open the **Home Page** and use the navigation buttons to select a report page.
2. Choose a date range or category in the slicers.
3. Hover over charts to view tooltips.
4. Select a chart segment to cross-filter other visuals, if interactions are enabled.
5. Use **Clear selections** on a slicer (or the eraser icon) to reset that filter.
6. Compare pages using the same filter context for consistent analysis.

## Notes and Limitations

- The report reflects the fields and values present in the supplied dataset; it does not independently verify ride events.
- Blank ratings and cancellation fields may indicate that the corresponding event did not occur or that the value was not recorded.
- “Booking Value” is the dataset's recorded value. Treat it as revenue only according to the project's business definition.
- Confirm the exact DAX measures and Power Query applied steps in the PBIX before publishing this README as a final technical record.

## Files

- `ncr_ride_bookings.csv` — source dataset
- `Uber.pbix` — Power BI report
- `README.md` — project and dashboard documentation
