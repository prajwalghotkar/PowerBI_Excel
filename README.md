# PowerBI_Excel

-----
# Power BI Practice Activity  – UK House Price Index (HPIAdmins)

Solution guide for the 8 tasks in `9. Practice Activity Number 2.html`, plus an interactive dashboard template.

## 1. What is in the data (checked from your workbook)

| Item | Value |
|---|---|
| Workbook | `5_2_SourceDataf_xlsx.xlsx` (17 sheets; you only need **HPIAdmins**) |
| Rows / columns | 1,584 rows × 26 columns |
| Granularity | One row per **month per region** (Jan 1995 – Dec 2016 = 264 months × 6 regions) |
| Regions (6) | Greater Manchester, Merseyside, South Yorkshire, Tyne and Wear, West Midlands, West Yorkshire |
| Source column names | `Date`, `RegionName` (no space), `Index`, `12m%Change` |
| Blanks | `12m%Change` is blank for the first 12 months of every region (all of 1995) = 72 blanks. `Index` has no blanks. |

**Analyst notes**
- Because 1995 has no `12m%Change`, the line chart starts at **1996**. This is correct, not an error. Power BI's Average ignores blanks.
- `12m%Change` already holds percentages as plain numbers (e.g. `4.68` means 4.68%). Keep it a **Decimal number**, NOT the Percentage type, or Power BI will multiply by 100.
- `Index` is a price index (≈30 in 1995, ≈100+ in 2016). Summing it is meaningless, so always use **Average**.

---

## 2. Step-by-step solution

### Task 1 – Connect to the HPIAdmins sheet
1. Open Power BI Desktop → **Home → Get data → Excel workbook**.
2. Select `SourceData` workbook (`5_2_SourceDataf_xlsx.xlsx`) → **Open**.
3. In Navigator tick **HPIAdmins** only (ignore the other 16 sheets).
4. Click **Transform Data** (not Load), because Tasks 3 and 4 are easiest in Power Query.

### Task 3 (do now, while in Power Query) – Data types
In Power Query Editor click each column header icon, or **Transform → Data Type**:

| Column | Type to set |
|---|---|
| Date | **Date** (not Date/Time) |
| RegionName | **Text** (this is the "String" field) |
| Index | **Decimal Number** |
| 12m%Change | **Decimal Number** |

Click **Replace current** if asked.

### Task 4a – Rename 12m%Change
Still in Power Query: right-click `12m%Change` → **Rename** → `12 month percentage change`.
Also rename `RegionName` → `Region Name` so it matches the assignment wording.

Then **Home → Close & Apply**.

### Task 2 – Hide every field except 4
1. Click **Model view** (left rail, third icon).
2. In the HPIAdmins table click `AreaCode`, then **Ctrl+click** every other column except **Date, Region Name, Index, 12 month percentage change**. (Tip: click first, Shift+click last, then Ctrl+click to deselect the four you keep.)
3. In the **Properties** pane set **Is hidden = Yes**.
4. Return to **Report view**: the Data pane should show only 4 fields.

### Task 4b – Default aggregation = Average
1. In Report view click `12 month percentage change` in the Data pane.
2. **Column tools** ribbon → **Summarization → Average**.
3. (Recommended) Do the same for **Index** → Average, which also makes Task 8 automatic.

### Task 5 – Line chart: average 12-month change per year, per region
1. Insert a **Line chart**.
2. **X-axis**: drag `Date`. Power BI creates a Date Hierarchy (Year, Quarter, Month, Day).
   Click the **X** next to Quarter, Month and Day so only **Year** remains. (If you only see "Date", click the dropdown on the field and choose **Date Hierarchy**.)
3. **Y-axis**: drag `12 month percentage change` (should read *Average of 12 month percentage change*).
4. **Legend**: drag `Region Name`.
5. Format → **X-axis → Type = Categorical** so each year is one point.
6. Title: *Average 12-month price change per year by region*.

> If Auto date/time is switched off (File → Options → Current file → Data Load), add a helper column instead: Power Query → **Add Column → Date → Year → Year**, set to Whole number, and use it as the X-axis.

### Task 6 – Color lines by Region Name
Putting `Region Name` in **Legend** already gives each region its own color. To control the colors: **Format → Visuals → Lines → Colors** (one swatch per region), or import the theme in section 4.

### Task 7 – Duplicate and change to "Circle display"
"Circle display" is not an official Power BI name. The built-in visual that draws circles is the **Scatter chart**, and it is also the one with a **Size** well, which Task 8 needs.
1. Click the line chart → **Ctrl+C**, **Ctrl+V** (pastes an identical copy).
2. With the copy selected, choose **Scatter chart** in the Visualizations pane.
3. Fix the field wells (scatter needs a numeric X):

| Well | Field |
|---|---|
| Values | `Year` (helper column, see below) |
| Legend | `Region Name` |
| X Axis | `Year` → dropdown → **Don't summarize** |
| Y Axis | `12 month percentage change` (Average) |

**Helper Year column:** Home → Transform data → select `Date` → **Add Column → Date → Year → Year** → type Whole number → rename `Year` → Close & Apply. Keep this one visible; it is a helper for the scatter only.

### Task 8 – Size = Average of Index
1. Drag `Index` into the **Size** well.
2. Click its dropdown → make sure it says **Average of Index** (not Sum).
3. Turn on **Format → Markers → Size** adjustments if bubbles are too large.

---

## 3. Expected values (use these to check your visuals)

Average of 12 month percentage change per year:

| Year | Greater Manchester | Merseyside | South Yorkshire | Tyne and Wear | West Midlands | West Yorkshire |
|---|---|---|---|---|---|---|
| 1996 | -0.31 | 0.89 | -0.17 | -0.37 | 0.63 | -0.43 |
| 2000 | 8.06 | 6.76 | 5.78 | 5.25 | 10.66 | 7.05 |
| 2007 | 8.62 | 5.74 | 7.33 | 6.40 | 5.42 | 8.37 |
| 2008 | -2.65 | -3.10 | -2.40 | -3.53 | -3.56 | -2.89 |
| 2009 | -10.63 | -8.88 | -9.60 | -9.06 | -9.43 | -9.77 |
| 2010 | 2.48 | 2.26 | 2.38 | 2.18 | 5.06 | 3.62 |
| 2016 | 7.05 | 4.45 | 4.19 | 3.73 | 7.59 | 5.47 |

Average of Index (bubble size): 2009 → GM 91.8, Merseyside 99.6, S Yorks 95.1, Tyne & Wear 98.1, W Midlands 89.2, W Yorks 95.3. 2016 → all between 106 and 110.

**Story the charts tell:** every region peaked around 2000–2007, fell sharply in 2008–2009 (financial crisis, about −9% to −11% in 2009), and recovered from 2010. West Midlands and Greater Manchester led the 2016 recovery.

---

## 4. Interactive dashboard template

### Page layout (16:9, 1280 × 720)

```
+---------------------------------------------------------------+
|  TITLE: UK House Price Index – Northern & Midlands Regions    |
+----------+----------------------------------------------------+
| SLICERS  |  KPI CARD 1      KPI CARD 2      KPI CARD 3        |
| Region   |  Avg 12m %       Avg Index       Latest Year       |
| (list)   +----------------------------------------------------+
| Year     |  [Line chart]            |  [Scatter / circle]     |
| (between)|  Avg 12m % per year      |  Size = Avg Index       |
| [Reset]  |  Legend = Region         |  Legend = Region        |
+----------+----------------------------------------------------+
|  Optional: matrix (Year x Region) of Avg 12m % with colors    |
+---------------------------------------------------------------+
```

### Interactivity checklist
1. **Slicer – Region Name**: Insert → Slicer → field `Region Name` → Format → Slicer settings → *List* or *Dropdown*, enable **Multi-select** (Ctrl+click) and **Select all**.
2. **Slicer – Year**: use `Year` helper column, style **Between** (slider).
3. **Cross-filtering**: click a line or bubble and the other visual highlights the same region. Fine-tune with **Format → Edit interactions**.
4. **Tooltips**: add `Index` and `Average price` style fields in the visual's **Tooltips** well; hover shows more detail.
5. **Bookmarks + buttons (toggle view)**: View → Bookmarks. Save one bookmark with only the line chart visible and another with only the scatter visible. Insert → Buttons → Blank ×2, set **Action → Bookmark**. The user can switch chart type with one click.
6. **Reset button**: bookmark taken with no filters applied, linked to a *Clear all slicers* button.
7. **Drill down**: on the line chart turn on drill mode (down arrow) to move from Year → Quarter → Month. Keep the hierarchy for this.
8. **Sync slicers** (View → Sync slicers) if you add a second page.

### Optional DAX measures (for cards and titles)
```DAX
Avg 12m Change = AVERAGE ( HPIAdmins[12 month percentage change] )

Avg Index = AVERAGE ( HPIAdmins[Index] )

Latest Year = MAX ( HPIAdmins[Year] )

Dynamic Title =
"Average 12-month change – "
    & IF ( ISFILTERED ( HPIAdmins[Region Name] ),
           CONCATENATEX ( VALUES ( HPIAdmins[Region Name] ), HPIAdmins[Region Name], ", " ),
           "All regions" )
```
Use `Dynamic Title` in **Format → Title → fx** so the heading changes with the slicer.

### Theme
Import `HPI_Theme.json` (View → Themes → Browse for themes). It gives six distinct, color-blind-friendly region colors, so Task 6 looks consistent across both visuals.

---

## 5. Common problems

| Problem | Fix |
|---|---|
| Lines start in 1996 | Expected: 1995 has no 12-month change. |
| Y-values look 100× too big or small | Column was set to Percentage; change to Decimal number. |
| X-axis shows months, not years | Remove Quarter/Month/Day from the hierarchy, or use the `Year` helper column. |
| Size looks huge or flat | Check it says *Average of Index*, not *Sum*. |
| Scatter shows only one dot per region | Put `Year` in **Values** (Details), not only in X Axis. |
| Date Hierarchy missing | File → Options → Current file → Data Load → enable Auto date/time, or use helper Year. |

## 6. Submission checklist
- [ ] Only Date, Region Name, Index, 12 month percentage change visible in Data pane
- [ ] Types: Date / Text / Decimal / Decimal
- [ ] Column renamed and aggregation = Average
- [ ] Line chart (per year, legend = Region Name, colored)
- [ ] Duplicate scatter chart with Size = Average of Index
- [ ] Slicers, bookmarks and reset button working
- [ ] Save as `PracticeActivity2.pbix`
