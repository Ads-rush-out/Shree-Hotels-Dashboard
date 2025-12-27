# 🏨 Hotel Analytics Dashboard - DAX Measures & Calculations

---

## 📊 Overview

This repository contains all DAX measures, calculated columns, and date table configurations used in the Hotel Analytics Dashboard. These measures enable comprehensive analysis of hotel operations including occupancy rates, revenue metrics, booking patterns, and performance KPIs.

---

## 🎯 Key Performance Indicators (KPIs)

### 1️⃣ Occupancy Metrics

#### Total Occupied Rooms
Counts the number of rooms currently checked-in.
```dax
Total Occupied Rooms = 
CALCULATE(
    COUNTROWS('Hotel Data'),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

#### Total Rooms Booked
Total count of all booking records.
```dax
Total Rooms Booked = 
CALCULATE(COUNTROWS('Hotel Data'))
```

#### Occupancy Rate
Percentage of booked rooms that are currently occupied.
```dax
Occupancy Rate = 
[Total Occupied Rooms] / [Total Rooms Booked]
```
**Format:** Percentage  
**Purpose:** Core KPI for hotel performance monitoring

---

### 2️⃣ Booking Analytics

#### Total Cancelled Bookings
Count of all cancelled reservations.
```dax
Total Cancelled Bookings = 
CALCULATE(
    COUNTROWS('Hotel Data'),
    'Hotel Data'[BookingStatus] = "Cancelled"
)
```

#### Total Days of Booking
Sum of all nights stayed by checked-in guests.
```dax
Total Days of Booking = 
CALCULATE(
    SUM('Hotel Data'[Duration]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

#### Average Day of Stay
Average length of stay across all bookings.
```dax
Average Day of Stay = 
AVERAGE('Hotel Data'[Duration])
```

---

### 3️⃣ Revenue Metrics

#### Total Revenue
Total revenue from checked-in bookings only.
```dax
Total Revenue = 
CALCULATE(
    SUM('Hotel Data'[Rev]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

#### Average Daily Rate (ADR)
Average room rate per night.
```dax
Average Daily Rate (ADR) = 
CALCULATE(AVERAGE('Hotel Data'[ADR]))
```
**Definition:** Standard hospitality metric measuring average rental income per occupied room

#### RevPAR (Revenue Per Available Room)
Key profitability metric combining occupancy and ADR.
```dax
RevPAR = 
[Average Daily Rate (ADR)] * [Occupancy Rate]
```
**Formula:** ADR × Occupancy Rate  
**Purpose:** Measures revenue generation efficiency

---

### 4️⃣ Night-Type Analysis

#### Total Weekend Nights
Sum of weekend nights for checked-in guests.
```dax
Total Weekend Nights = 
CALCULATE(
    SUM('Hotel Data'[Stays In Weekend Nights]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

#### Total Weekday Nights
Sum of weekday nights for checked-in guests.
```dax
Total Weekday Nights = 
CALCULATE(
    SUM('Hotel Data'[Stays In Week Nights]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

---

### 5️⃣ Revenue Breakdown by Night Type

#### Week Night Revenue (Calculated Column)
Revenue generated from weekday nights per booking.
```dax
Week Night Revenue = 
'Hotel Data'[Week Nights] * 'Hotel Data'[ADR]
```

#### Weekend Night Revenue (Calculated Column)
Revenue generated from weekend nights per booking.
```dax
Weekend Night Revenue = 
'Hotel Data'[Weekend Nights] * 'Hotel Data'[ADR]
```

#### Week Night Rev (Measure)
Total weekday night revenue for checked-in bookings.
```dax
Week Night Rev = 
CALCULATE(
    SUM('Hotel Data'[Week Night Revenue]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

#### Weekend Night Rev (Measure)
Total weekend night revenue for checked-in bookings.
```dax
Weekend Night Rev = 
CALCULATE(
    SUM('Hotel Data'[Weekend Night Revenue]),
    'Hotel Data'[BookingStatus] = "Checked-In"
)
```

---

### 6️⃣ Weekly Performance Metrics

#### Average Week Revenue
Average weekly revenue across all weeks, ignoring date filters.
```dax
Average Week Revenue = 
CALCULATE(
    AVERAGEX(
        VALUES('Data Table'[Week]),
        CALCULATE(SUM('Hotel Data'[REV]))
    ),
    ALL('Date Table')
)
```
**Context:** Uses ALL() to calculate baseline average across entire dataset

#### Average Week Occupied Rooms
Average number of occupied rooms per week.
```dax
Average Week Occupied Rooms = 
CALCULATE(
    AVERAGEX(
        VALUES('Data Table'[Week]),
        CALCULATE([Total Occupied Rooms])
    ),
    ALL('Date Table')
)
```

---

## 📅 Date Table Configuration

### Date Table Creation
Creates a continuous date table from first to last check-in date.
```dax
Date Table = 
CALENDAR(
    MIN('Hotel Data'[CheckInDate]),
    MAX('Hotel Data'[CheckInDate])
)
```

### Date Hierarchy Columns

#### Month Name
```dax
Month = 
FORMAT('Date Table'[Date], "mmmm")
```
**Output:** January, February, March, etc.

#### Month Number
```dax
Month Number = 
MONTH('Date Table'[Date])
```
**Output:** 1, 2, 3, ..., 12

#### Quarter
```dax
Quarter = 
QUARTER('Date Table'[Date])
```
**Output:** 1, 2, 3, 4

#### Quarter Name
```dax
Quarter Name = 
CONCATENATE("Q", 'Date Table'[Quarter])
```
**Output:** Q1, Q2, Q3, Q4

#### Year
```dax
Year = 
YEAR('Date Table'[Date])
```

#### Week Number
```dax
Week = 
WEEKNUM('Date Table'[Date])
```
**Output:** 1-52 (week number of the year)

---

## 🏗️ Measure Categories

### Core KPIs
| Measure | Category | Format |
|---------|----------|--------|
| Total Occupied Rooms | Occupancy | Whole Number |
| Total Rooms Booked | Occupancy | Whole Number |
| Occupancy Rate | Occupancy | Percentage |
| Total Revenue | Revenue | Currency |
| Average Daily Rate (ADR) | Revenue | Currency |
| RevPAR | Revenue | Currency |

### Operational Metrics
| Measure | Category | Format |
|---------|----------|--------|
| Total Days of Booking | Operations | Whole Number |
| Total Weekend Nights | Operations | Whole Number |
| Total Weekday Nights | Operations | Whole Number |
| Total Cancelled Bookings | Operations | Whole Number |
| Average Day of Stay | Operations | Decimal (1-2 places) |

### Revenue Analysis
| Measure | Category | Format |
|---------|----------|--------|
| Week Night Rev | Revenue Breakdown | Currency |
| Weekend Night Rev | Revenue Breakdown | Currency |
| Average Week Revenue | Trends | Currency |
| Average Week Occupied Rooms | Trends | Decimal |

---

## 💡 Business Insights Enabled

### 📈 Performance Tracking
- **Occupancy Rate**: Monitor hotel capacity utilization
- **RevPAR**: Evaluate revenue efficiency and pricing strategy
- **ADR**: Track average room pricing trends

### 💰 Revenue Optimization
- **Weekend vs Weekday Revenue**: Identify peak revenue periods
- **Weekly Trends**: Spot seasonal patterns and anomalies
- **Cancellation Analysis**: Measure booking reliability

### 📊 Operational Intelligence
- **Average Length of Stay**: Understand guest behavior
- **Night Type Analysis**: Optimize weekend/weekday pricing
- **Weekly Averages**: Benchmark current performance against historical data

---

## 🔧 Implementation Guide

### Step 1: Create Date Table
1. Go to **Modeling** tab in Power BI
2. Click **New Table**
3. Paste the Date Table DAX formula
4. Mark as Date Table (Table Tools → Mark as Date Table)

### Step 2: Add Calculated Columns
1. Select 'Hotel Data' table
2. Click **New Column**
3. Add `Week Night Revenue` and `Weekend Night Revenue` columns

### Step 3: Create Measures
1. Select 'Hotel Data' table or create a dedicated Measures table
2. Click **New Measure**
3. Add each measure one by one
4. Organize into Display Folders for better organization

### Step 4: Format Measures
- **Currency**: Total Revenue, ADR, RevPAR, Week Night Rev, Weekend Night Rev
- **Percentage**: Occupancy Rate (format as %, 2 decimal places)
- **Whole Number**: Room counts, night counts
- **Decimal**: Average Day of Stay (1-2 decimal places)

---

## 📋 Measure Naming Conventions

| Pattern | Example | Purpose |
|---------|---------|---------|
| `Total [Metric]` | Total Revenue | Aggregated count or sum |
| `Average [Metric]` | Average Daily Rate | Mean calculation |
| `[Metric] Rate` | Occupancy Rate | Ratio or percentage |
| `[Time] [Metric]` | Week Night Rev | Time-specific calculation |

---

## 🎓 DAX Techniques Demonstrated

### ✅ Context Manipulation
- **CALCULATE()**: Modifying filter context
- **ALL()**: Removing filters for baseline calculations
- **VALUES()**: Obtaining distinct values

### ✅ Aggregation Functions
- **SUM()**: Total revenue and night counts
- **AVERAGE()**: ADR and length of stay
- **COUNTROWS()**: Booking and room counts

### ✅ Iterator Functions
- **AVERAGEX()**: Row-by-row average calculations

### ✅ Time Intelligence
- **CALENDAR()**: Date table generation
- Date hierarchy columns for drill-down analysis

### ✅ Text Functions
- **FORMAT()**: Date formatting
- **CONCATENATE()**: String combination

---

## 📊 Suggested Visualizations

### Dashboard Layout Recommendations

**KPI Cards:**
- Occupancy Rate
- RevPAR
- Total Revenue
- ADR

**Line Charts:**
- Occupancy Rate over time
- Revenue trends (Week/Month/Quarter)

**Bar Charts:**
- Week Night Rev vs Weekend Night Rev
- Cancelled vs Checked-In bookings

**Tables:**
- Weekly performance breakdown
- Month-over-month comparison

---

## 🔮 Future Enhancements

- [ ] Add Year-over-Year (YoY) comparison measures
- [ ] Create rolling averages for trend analysis
- [ ] Implement forecasting measures
- [ ] Add customer segmentation metrics
- [ ] Create dynamic targets and variance calculations
- [ ] Add seasonality index calculations

---

### Key Terms:
- **ADR (Average Daily Rate)**: Average rental revenue per occupied room
- **RevPAR (Revenue Per Available Room)**: ADR × Occupancy Rate
- **Occupancy Rate**: Percentage of available rooms that are occupied

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

- GitHub: https://github.com/Ads-rush-out
- Email: adarshkumarrout01@gmail.com
- Linkedin: www.linkedin.com/in/adarsh-rout

---
