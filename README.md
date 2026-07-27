# Education, Technology & Economic Growth: Africa vs. Rest of the World


An end-to-end data analytics project examining whether education and technology adoption are linked to economic growth, comparing Sub-Saharan Africa against the rest of the world using World Bank data (2000–2023).

<img width="1920" height="1080" alt="AnalystLab Africa Capstone Project" src="https://github.com/user-attachments/assets/7ac0dbe0-1946-43c0-80ea-2503a056d250" />

---

## 📌 Project Overview

Education and digital access are widely regarded as key drivers of economic development, yet the pace of progress on both fronts varies significantly across regions. This project investigates whether countries with higher levels of education and greater technology adoption experience stronger economic outcomes, with a specific focus on comparing **Africa (Sub-Saharan Africa)** against the **rest of the world**.

The project follows the complete data analytics workflow — data acquisition, cleaning, modeling, analysis, visualization, and interpretation — applied to real-world World Bank data, and communicates the results through an interactive Power BI dashboard and a written report.

- **Objective:** Assess the relationship between education, internet/technology adoption, and GDP per capita, and compare economic growth patterns between Africa and the rest of the world.
- **Data Source:** [World Bank World Development Indicators (WDI)](https://datatopics.worldbank.org/world-development-indicators/)
- **Scope:** 217 countries, 9 indicators across 3 themes (Education, Technology, Economic), years 2000–2023
- **Regional grouping:** Africa (Sub-Saharan Africa, 48 countries) vs. Rest of the World (169 countries)

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Python (pandas)** | Data cleaning, filtering, unpivoting, joining, and validation of the raw WDI dataset |
| **Google Colab** | Cloud environment used to run the Python cleaning script, chosen for its ability to handle the large raw CSV (~400,000 rows) without local performance issues |
| **Google Drive** | Storage for raw and cleaned datasets, connected to Colab via `drive.mount()` for persistent access across sessions |
| **Power BI Desktop** | Data modeling (star schema relationships), DAX measure creation, and interactive dashboard development |
| **DAX (Data Analysis Expressions)** | Custom measures for regional averages, Pearson correlation coefficients, and GDP growth calculations |
| **Power BI Key Influencers** | Built-in AI-powered visual used to identify which indicators most strongly predict higher GDP per capita |
| **Microsoft Word / docx** | Final written report (objective, methodology, findings, insights, recommendations) |
| **GitHub** | Version control and public hosting of the project files, code, and documentation |
| **LinkedIn** | Publishing the completed project for professional visibility (`#AnalystLabAfrica`) |

---

## 🗂️ Repository Structure

```
├── data/
│   ├── wdi_indicators_clean.csv   # Cleaned fact table (Country + Year + 9 indicators)
│   └── country_lookup.csv         # Country dimension table (Region, Income Group, Region_Group)
├── notebooks/
│   └── Week_8_Capstone_Project.ipynb        # Python cleaning script (Google Colab)
├── dashboard/
│   ├── Capstone_Project_AnalystLab_Africa.pbix    # Power BI dashboard file
│   └── screenshots/               # Dashboard page screenshots
├── report/
│   └── Capstone_Project_Report.docx       # Final written report
└── README.md
```

---

## 📊 Indicators Used

| Theme | Indicator | WDI Code |
|---|---|---|
| Education | Literacy rate, adult total (% ages 15+) | `SE.ADT.LITR.ZS` |
| Education | School enrollment, secondary (% gross) | `SE.SEC.ENRR` |
| Education | School enrollment, tertiary (% gross) | `SE.TER.ENRR` |
| Technology | Individuals using the Internet (% of population) | `IT.NET.USER.ZS` |
| Technology | Mobile cellular subscriptions (per 100 people) | `IT.CEL.SETS.P2` |
| Technology | Fixed broadband subscriptions (per 100 people) | `IT.NET.BBND.P2` |
| Economic | GDP per capita (current US$) | `NY.GDP.PCAP.CD` |
| Economic | GDP growth (annual %) | `NY.GDP.MKTP.KD.ZG` |
| Economic | Unemployment, total (% of labor force) | `SL.UEM.TOTL.ZS` |

---

## 🧹 Data Cleaning Process

Cleaning was performed in **Python (pandas)** via **Google Colab**, rather than Power Query, for two reasons: the raw file (~400,000 rows) is large enough that Power BI's Power Query can be slow to work with before it's filtered down, and a documented Python script doubles as a second, more substantial artifact for this repository. Full script: [`notebooks/Week_8_Capstone_Project.ipynb`](./notebooks/Week_8_Capstobe_Project.ipynb).

| Step | Action | Resulting Rows |
|---|---|---|
| 1 | Loaded raw `WDICSV.csv` and `WDICountry.csv` from Google Drive | 396,970 |
| 2 | Filtered to the 9 selected indicator codes | 2,385 |
| 3 | Unpivoted (melted) wide year-columns into long format with `Year` and `Value` columns | 157,410 |
| 4 | Restricted to years 2000–2023 and dropped rows with missing `Value` | 44,683 |
| 5 | Removed regional/income aggregates (e.g., "World," "Sub-Saharan Africa" as an aggregate, "Arab World") by keeping only rows whose `Country Code` had a non-null `Region` in `WDICountry.csv` | 35,690 (217 real countries) |
| 6 | Checked for and removed duplicate Country–Indicator–Year rows | 0 duplicates found |
| 7 | Pivoted the cleaned long-format table into a wide fact table (one row per Country + Year, 9 indicators as columns) | 5,176 |
| 8 | Built a separate country lookup table (`Country Code`, `Country Name`, `Region`, `Income Group`) and derived a `Region_Group` field | 217 |
| 9 | Validated referential integrity — confirmed every `Country Code` in the fact table has a matching row in the lookup table | 0 mismatches |
| 10 | Exported two clean CSVs: `wdi_indicators_clean.csv` (fact table) and `country_lookup.csv` (dimension table) | — |

**Region_Group definition:** `Africa` = Sub-Saharan Africa (48 countries); `Rest of World` = all other regions (169 countries).

**Note on scope:** "Africa" is defined strictly as Sub-Saharan Africa per WDI's regional classification. North African countries (Egypt, Morocco, Algeria, etc.) fall under WDI's "Middle East & North Africa" region and are therefore grouped under "Rest of World" in this analysis. This was a deliberate scoping decision, documented here for transparency rather than treated as a data limitation to hide.

---

## 📈 Dashboard

Built in **Power BI Desktop**. The data model uses a star schema: `wdi_indicators_clean` (fact table) is connected to `country_lookup` (dimension table) via a many-to-one relationship on `Country Code`, with single-direction cross-filtering. This allows slicers built on `country_lookup` (Region, Income Group, Region_Group) to filter every visual built from the indicators table.

### Page 1 — Executive Overview
- KPI cards: Avg GDP per Capita, GDP Growth %, Country Count, Avg Internet Users %, Literacy Rate
- World map (bubble map) sized by Avg GDP per Capita
- Bar chart: Avg Internet Users % by Region
- Bar chart: Avg GDP per Capita by Region
- Donut chart: Country Count by Region_Group (Africa vs. Rest of World)
- Slicers: Year, Region_Group

### Page 2 — Education & Technology Trends
- KPI cards: Avg Secondary Enrollment %, Avg Tertiary Enrollment %, Literacy Rate, Mobile Subscription %
- Line chart: Average Internet Users % trend (2000–2023), Africa vs. Rest of World
- Line chart: Average Secondary Enrollment % trend (2000–2023), Africa vs. Rest of World
- Bar chart: Top 10 countries by Tertiary Enrollment %
- Bar chart: Mobile Subscription by Region
- Slicers: Year, Income Group

### Page 3 — Correlation & Growth
- KPI cards: Correlation (Education ↔ GDP), Correlation (Internet ↔ GDP), GDP Growth %, Avg GDP per Capita
- Bar chart: GDP Growth %, Africa vs. Rest of World
- Line chart: GDP Growth Trend by Region (2000–2023)
- Scatter chart: Internet Users % vs. GDP per Capita, colored by Region_Group
- **Key Influencers visual**: analyzes what drives GDP per capita to increase, using all 9 indicators as explanatory factors
- Slicers: Decade, Region_Group

Each page includes a short written insight caption summarizing the key takeaway from that page's visuals.

See `dashboard/screenshots/` for page previews and `dashboard/Capstone_Project_AnalystLab_Africa.pbix` for the interactive file.

---

## 🔑 Key Findings

- Global average GDP per capita is ~$16,074; Sub-Saharan Africa sits at ~$2K, far behind North America (~$65K).
- Internet adoption in Sub-Saharan Africa (14%) trails far behind the global average (38.7%) and North America (77%).
- Internet access correlates more strongly with GDP per capita (**r = 0.51**) than secondary education does (**r = 0.21**).
- Power BI's Key Influencers analysis flagged mobile subscription rates and internet usage as the strongest predictors of higher GDP per capita.
- Africa's average GDP growth rate (4.05%) exceeds the rest of the world's (3.13%), but this hasn't been enough to close the per-capita income gap, since Africa is growing from a much smaller base.

Full findings, insights, and recommendations are in [`report/Capstone_Project_Report.docx`](./report/Capstone_Project_Report.docx).

---

## ▶️ How to Reproduce

1. Download the WDI bulk CSV from the [World Bank WDI portal](https://datatopics.worldbank.org/world-development-indicators/).
2. Run `notebooks/Week_8_Capstone_Project.ipynb` (update file paths to your local/Drive location).
3. Open `dashboard/Capstone_Project_AnalystLab_Africa.pbix` in Power BI Desktop and refresh the data source to point to your cleaned CSVs.

---


## 🔗 Connect

Built by Vivian Okoaze as part of the AnalystLab Africa Data Analytics Internship.
[LinkedIn](https://www.linkedin.com/in/vivian-okoaze-108369173) • [X / Twitter](https://x.com/Vivian_Okoaze)

`#AnalystLabAfrica`
