# 🏨 Bodrum Azure Coast Hotel — Analytics & Revenue Dashboard

An end-to-end Power BI analytics solution designed for hotel operations and revenue managers. This interactive 3-page dashboard transforms raw booking, room, and guest operational data into actionable business intelligence to optimize room yields, track booking channels, and profile guest demographics.

---

## 📋 Executive Summary

The **Bodrum Azure Coast Hotel Analytics Dashboard** provides management with a clear, single-source-of-truth view of business performance. By modeling multi-source operational data, this tool helps evaluate channel profitability, monitor peak occupancy cycles, and profile high-value guest demographics to drive revenue growth.

---

## 📌 Dashboard Pages

### Page 1 — Executive Overview
| Visual | Description |
|--------|-------------|
| **KPI Cards** | Total Realized Revenue · Occupancy Rate % · ADR · RevPAR — each with YoY growth % and directional arrows |
| **Monthly Trend** | Line & stacked column combo chart — seasonal demand spikes and monthly income |
| **Booking Status** | Donut chart — Completed · Confirmed · Cancelled distribution |
| **Room Type Performance** | Clustered bar chart — revenue by room category |
| **Booking Channel Breakdown** | Clustered column chart — OTA vs. direct channel revenue |

---

### Page 2 — Revenue & Channel Performance
| Visual | Description |
|--------|-------------|
| **Financial KPIs** | Gross Revenue · Cancellation Loss · Cancellation Fee · Average Stay Nights |
| **Revenue Streams** | Monthly trend — Room Revenue · F&B · Other Services |
| **Promotion Performance** | Column chart — Early Bird · Long Stay · Direct Booking campaign returns |
| **Channel Yield Matrix** | Pivot table — Bookings · Revenue · ADR · Stay Length · Cancellation Rate by channel |

---

### Page 3 — Guest Insights & Demographics
| Visual | Description |
|--------|-------------|
| **Demographic KPIs** | Total Guests · Top Country · Returning Guests · Average Adults/Room |
| **Geographic Origin** | Bar chart — top revenue-generating nationalities |
| **Loyalty Tier Distribution** | Donut chart — guest engagement across Loyalty Tiers 1–5 |
| **Decomposition Tree** | Breakdown of revenue by nationality, language, and loyalty level |
| **Demographic Matrix** | Pivot table — bookings · revenue · ADR by language and loyalty tier |

---

## 🗃️ Data Model — Star Schema

```
                    dim_calendar
                         │
dim_guests ──── fact_bookings ──── dim_room_types
                         │
              fact_daily_room_operations
                         │
                   dim_promotions
```

### Fact Tables
| Table | Description |
|-------|-------------|
| `fact_bookings` | Individual reservation transactions, channel sources, room rates, financial metrics |
| `fact_daily_room_operations` | Daily capacity logs, available room counts, occupied space tracking |

### Dimension Tables
| Table | Description |
|-------|-------------|
| `dim_guests` | Customer demographics, nationality, loyalty tiers, language preferences |
| `dim_room_types` | Room categories, base rates, view types (Sea View, Land View, Partial View) |
| `dim_promotions` | Special offer codes, campaign parameters, applied discounts |
| `dim_calendar` | Dedicated Date dimension supporting all time-intelligence DAX functions |

---

## 📐 Key DAX Measures

### Measures Table 1 — Core KPIs
```dax
Total Realized Revenue = SUM(fact_bookings[realized_revenue_try])

Occupation Rate = 
DIVIDE(
    SUM(fact_daily_room_operations[occupied_rooms]),
    SUM(fact_daily_room_operations[available_rooms])
)

ADR = 
DIVIDE([Total Realized Revenue], 
    COUNTROWS(FILTER(fact_bookings, fact_bookings[booking_status] = "Completed"))
)

RevPAR = 
DIVIDE([Total Realized Revenue], 
    SUM(fact_daily_room_operations[available_rooms])
)
```

### Measures Table 2 — Revenue & Channel
```dax
Gross Revenue = 
    SUM(fact_bookings[room_revenue_try]) + 
    SUM(fact_bookings[food_beverage_revenue_try]) + 
    SUM(fact_bookings[other_revenue_try])

Cancellation Loss = SUM(fact_bookings[cancellation_fee_try])

Average Stay Nights = 
DIVIDE(SUM(fact_bookings[length_of_stay]), [Total Booking])
```

### Measures Table 3 — Guest Demographics
```dax
Total Guests = COUNTROWS(dim_guests)

Returning Guests = 
CALCULATE(COUNTROWS(fact_bookings), fact_bookings[is_returning] = TRUE())

Average Adult = DIVIDE(SUM(fact_bookings[adults]), [Total Booking])
```

### Display Table — KPI Growth Indicators
```dax
Revenue Growth Display = 
VAR GrowthPct = [Revenue Growth %]
RETURN
IF(GrowthPct > 0, "↑ " & FORMAT(GrowthPct,"0.00%"), "↓ " & FORMAT(ABS(GrowthPct),"0.00%"))

Revenue Growth Color = IF([Revenue Growth %] > 0, "#00C853", "#FF1744")
```

> All growth indicators use `SAMEPERIODLASTYEAR()` via `dim_calendar` for accurate YoY comparisons.

---

## 🛠️ Tools & Technologies

| Tool | Usage |
|------|-------|
| **Power BI Desktop** | Dashboard design, data modeling, visualization |
| **Power Query (M)** | Data cleaning, transformation, null handling |
| **DAX** | 3 measure tables + 1 display table with 15+ custom measures |
| **Star Schema** | 2 fact tables + 4 dimension tables |
| **Conditional Formatting** | Dynamic color coding for KPI growth indicators |
| **Bookmarks + Buttons** | Page navigation with professional sidebar |

---

## 🏨 Hotel KPIs Explained

| KPI | Formula | Insight |
|-----|---------|---------|
| **Occupancy Rate** | Occupied Rooms ÷ Available Rooms | Demand efficiency |
| **ADR** | Revenue ÷ Occupied Rooms | Pricing performance |
| **RevPAR** | Revenue ÷ Available Rooms | Overall yield |
| **Cancellation Rate** | Cancelled ÷ Total Bookings | Booking reliability |

---

## 🚀 How to Use

1. Download `hotel_end_to_end.pbix`
2. Open with **Power BI Desktop** (free)
3. Use **navigation buttons** to switch between the 3 pages
4. All KPIs update automatically with date filters
5. Growth arrows ↑ ↓ and colors update dynamically with data

---

## 💡 Key Business Insights This Dashboard Answers

```
✅ Which booking channel generates the highest revenue per booking?
✅ What is the seasonal occupancy pattern across the year?
✅ Which room types deliver the best revenue yield?
✅ What nationalities are our highest-value guests?
✅ How effective are our promotional campaigns?
✅ What is our cancellation loss vs. recovered fees?
```

---

## 👤 Author
**MOHAMMED AMIN BOUSSAKER**
**Data Analyst | IS Engineering Student | Muğla, Türkiye 🇹🇷**
Python · SQL · Power BI · DAX · Power Query · pandas · Statistical Testing
AR · FR · TR · EN

---

## 📁 Repository Structure

```
hotel-analytics-dashboard/
│
├── README.md
├── hotel_end_to_end.pbix
├── data/
│   └── (source data files)
└── images/
    ├── 
    ├── 
    └── <img width="1177" height="658" alt="Capture d&#39;écran 2026-08-26 232859" src="https://github.com/user-attachments/assets/fccebb46-c43e-4a32-b3ef-8c2583233d89" />
<img width="1082" height="616" alt="Capture d&#39;écran 2026-08-26 160042" src="https://github.com/user-attachments/assets/e772ddbb-52de-4750-a899-e9100b64188e" />
<img width="1095" height="616" alt="Capture d&#39;écran 2026-08-26 160200" src="https://github.com/user-attachments/assets/67a993f0-a009-4275-9207-b396300b52d5" />

```


