# Cybersecurity Operations Dashboard

**Executive Power BI Dashboard for Vulnerability Management & Compliance Tracking**

![Dashboard Preview](file://C:\Users\DataLab\OneDrive\Documents\DataProjects\02_PowerBI\Documentation\powerbi-cybersecurity-dashboard\screenshots\page1_overview.png?msec=1762807135585)

## 🔗 Live Dashboard

**[View Interactive Dashboard](https://app.powerbi.com/view?r=eyJrIjoiMjUzYTg5MGMtNWE4NC00NWM1LThiZmYtNjNmODI3YTk2ZDQxIiwidCI6ImRjNDcyNDc0LWU3MDctNDJiMS1hOTMxLTQyZTVlMjY0MGQ3MyJ9)

---

## Project Overview

A comprehensive 3-page Power BI dashboard providing real-time visibility into cybersecurity operations, vulnerability management, and compliance tracking across multi-site utility infrastructure. Built to support executive decision-making and operational prioritization in IT security operations.

**Built with:** Power BI Pro, DAX, Power Query, Star Schema Data Modeling

**Domain:** Cybersecurity Operations & Compliance Management

**Data Period:** January 2024 - October 2025 (Synthetic Data)

---

## Business Problem

IT security teams at enterprise organizations need:

- Real-time visibility into vulnerability status across hundreds of systems
- Trend analysis to identify if remediation efforts are keeping pace with discovery
- Compliance monitoring across multiple frameworks (SOC 2, ISO 27001, NIST)
- Executive-level reporting that highlights critical risks requiring immediate attention

This dashboard addresses these needs by consolidating vulnerability and compliance data into actionable insights for both technical teams and C-suite leadership.

---

## Dashboard Pages

### Page 1: Executive Summary

![Executive Summary](file://C:\Users\DataLab\OneDrive\Documents\DataProjects\02_PowerBI\Documentation\powerbi-cybersecurity-dashboard\screenshots\page1_overview.png?msec=1762807135585)

**Purpose:** High-level operational status for C-suite and security leadership

**Key Metrics:**

- **Current Status:** 90 Open, 85 In-Progress, 325 Closed vulnerabilities
- **MTTR:** 20 days average time to remediate
- **Critical Count:** 31 vulnerabilities requiring immediate attention
- **Severity Distribution:** 40% Medium, 30% High, 15% Critical, 15% Low

**Interactive Features:**

- Filter by Location (San Diego HQ, Remote Cloud, Branch Office)
- Filter by Remediation Owner (Security Team, IT Ops, Development, Infrastructure)
- Cross-filtering across all visuals

**Key Insights:**

- 78% closure rate (325 closed / 500 total) exceeds industry average of 65%
- Applications have highest vulnerability count (194 total, 26 Critical)
- 20-day MTTR meets enterprise SLA target of < 25 days
- Critical vulnerabilities represent 6% of total, within acceptable risk threshold

---

### Page 2: Vulnerability Management - Trend Analysis & At-Risk Systems

![Vulnerability Management](file://C:\Users\DataLab\OneDrive\Documents\DataProjects\02_PowerBI\Documentation\powerbi-cybersecurity-dashboard\screenshots\page2_overview.png?msec=1762807135587)

**Purpose:** Detailed operational view for security teams to identify trends, aging vulnerabilities, and prioritize remediation

**Visuals:**

1. **Severity Distribution** - Donut chart showing vulnerability composition
2. **Discovered vs Closed Trend** - Area chart revealing quarterly patterns
3. **Aging Analysis** - Stacked column chart showing time-to-remediate by severity
4. **Top At-Risk Systems** - Table with conditional formatting highlighting critical systems

**Key Insights:**

- Q1 2024 showed high discovery rate (195 vulnerabilities) during annual audit cycle
- Q3 2025 demonstrates improvement with discovery/closure rates converging
- 225 vulnerabilities aged 90+ days represent backlog requiring escalation
- CRM system has 5 Critical vulnerabilities with 415-day average age (highest priority)
- Payment Gateway and Patch Management systems also show concerning aging patterns

**Business Value:** Security team can immediately identify which systems need patching priority and which vulnerabilities have been aging without resolution.

---

### Page 3: Compliance Tracking - Framework Monitoring

![Compliance Tracking](file://C:\Users\DataLab\OneDrive\Documents\DataProjects\02_PowerBI\Documentation\powerbi-cybersecurity-dashboard\screenshots\page3_overview.png?msec=1762807135585)

**Purpose:** Framework compliance monitoring for audit preparation and risk management

**Visuals:**

1. **Overall Compliance Score** - KPI card showing 78% vs 80% target
2. **Compliance Trend** - Line chart showing quarterly progression (69% → 95% → 78%)
3. **Top Control Findings** - Matrix showing findings by framework and severity
4. **Compliance by Business Unit** - Bar chart identifying department-level gaps

**Key Insights:**

- Compliance peaked at 95% in Q3 2025, declined to 78% in Q4 (2.34% below target)
- Q4 decline reflects comprehensive year-end audit identifying 77 SOC 2 findings
- SOC 2 Type II has highest finding count (77 total, 38 Medium priority)
- Operations department shows lowest compliance rate (79%) requiring focused support
- 16 Critical findings across all frameworks require immediate remediation

**Business Value:** Audit committees can track compliance trajectory and prioritize remediation based on framework severity and business unit performance.

---

## Technical Implementation

### Data Architecture

**Data Model:** Star schema with fact and dimension tables

**Tables:**

- **Fact Table:** Vulnerabilities (500 rows)
  
  - Vulnerability_ID, Date_Discovered, System_ID, Severity, Status, Days_To_Remediate, etc.
- **Dimension Tables:**
  
  - Systems (30 rows) - System metadata and criticality
  - Compliance (200 rows) - Framework assessment data

**Relationships:** One-to-many from Systems → Vulnerabilities, enforcing referential integrity

---

### Key DAX Measures

```dax
// Vulnerability Status Metrics
Total Open = 
CALCULATE(
    COUNTROWS(Vulnerabilities),
    Vulnerabilities[Status] = "Open"
)

MTTR = 
CALCULATE(
    AVERAGE(Vulnerabilities[Days_To_Remediate]),
    Vulnerabilities[Status] = "Closed"
)

Critical Count = 
CALCULATE(
    COUNTROWS(Vulnerabilities),
    Vulnerabilities[Severity] = "Critical"
)

// Trend Analysis
Vulnerabilities Discovered = COUNT(Vulnerabilities[Vulnerability_ID])

Vulnerabilities Closed = 
CALCULATE(
    COUNT(Vulnerabilities[Vulnerability_ID]),
    Vulnerabilities[Status] = "Closed"
)

// Compliance Metrics
Compliance Score = 
DIVIDE(
    CALCULATE(COUNTROWS(Compliance), Compliance[Status] = "Compliant"),
    COUNTROWS(Compliance),
    0
)

Compliance vs Target = [Compliance Score] - 0.80
```

---

### Power BI Features Demonstrated

**Advanced Visualizations:**

- Stacked bar charts with severity breakdown
- Area charts for trend analysis
- Matrix visuals with hierarchical grouping
- Conditional formatting with gradient scales
- KPI cards with target benchmarking

**Interactivity:**

- Cross-page filtering via slicers
- Drill-through capabilities (Pages 2-3)
- Dynamic data labels
- Tooltip customization
- Mobile-optimized layouts

**Data Modeling:**

- Star schema design for optimal performance
- Date table with time intelligence functions
- Calculated columns for age bucketing
- Measures for dynamic aggregations

---

## Business Applications

This dashboard structure is adaptable for:

- IT Security Operations Centers (SOC)
- Risk Management Reporting
- Compliance Audit Preparation
- Executive Board Presentations
- Vendor Risk Management
- Cybersecurity Maturity Assessments

**Industries:** Utilities, Financial Services, Healthcare, Government, Technology

---

## Repository Structure

```
powerbi-cybersecurity-dashboard/
├── screenshots/
│   ├── page1_overview.png
│   ├── page1_critical_filter.png
│   ├── page2_overview.png
│   ├── page2_trend.png
│   ├── page3_overview.png
│   └── page3_matrix.png
├── data/
│   ├── vulnerabilities.csv
│   ├── systems.csv
│   └── compliance.csv
├── dashboard.pbix
└── README.md
```

---

## How to Use

### View Online

Click the **[Live Dashboard Link](https://app.powerbi.com/view?r=eyJrIjoiMjUzYTg5MGMtNWE4NC00NWM1LThiZmYtNjNmODI3YTk2ZDQxIiwidCI6ImRjNDcyNDc0LWU3MDctNDJiMS1hOTMxLTQyZTVlMjY0MGQ3MyJ9)** above to interact with the published dashboard.

### Download & Explore Locally

1. Download `dashboard.pbix` from this repository
2. Open with Power BI Desktop (free download from Microsoft)
3. Explore data model, DAX measures, and visual formatting
4. Modify with your own data using the same schema

### Replicate with Your Data

The `data/` folder contains sample CSV files showing the required schema. Replace with your organization's vulnerability and compliance data while maintaining column names and data types.

---

## Design Principles Applied

**Color Strategy:**

- Blue gradient palette for professional, corporate appearance
- Darker blue = higher priority/urgency (follows natural attention patterns)
- Conditional formatting guides eye to critical issues
- Minimal use of accent colors (orange for targets only)

**Information Hierarchy:**

- Page 1: Summary metrics (executive 30-second scan)
- Page 2: Operational details (security team daily use)
- Page 3: Compliance monitoring (audit preparation)

**Data Visualization Best Practices:**

- KPI cards for single-number metrics
- Trend lines for temporal analysis
- Stacked charts for composition
- Tables for detail drill-down
- All visualizations include clear labels and context

---

## Learning Outcomes

Building this dashboard demonstrates proficiency in:

**Technical Skills:**

- Power BI Desktop (data modeling, DAX, visualization design)
- Data modeling (star schema, relationships, calculated columns)
- Business intelligence (KPIs, trend analysis, conditional formatting)
- Data analysis (aging patterns, severity distribution, compliance gaps)

**Business Acumen:**

- Cybersecurity operations domain knowledge
- Risk-based prioritization thinking
- Executive communication (distilling technical data for leadership)
- Compliance framework understanding (SOC 2, ISO 27001, NIST)

**Soft Skills:**

- Translating business requirements into technical solutions
- Information design and visual communication
- Stakeholder-focused reporting

---

## About the Author

**Ernesto Gonzales, MSDA**  
Data Analyst | San Diego, CA

I'm a data analyst with 3+ years of experience in utilities and IT operations, specializing in Power BI, SQL, and Python. This dashboard reflects my approach to data analytics: translating complex technical operations into clear, actionable insights for decision-makers.

**Master's Degree:** Data Analytics, Western Governors University (2024)  
**Bilingual:** English/Spanish

**Connect:**

- 🌐 [Website](https://ernestogonzales.com)
- 💼 [LinkedIn](https://linkedin.com/in/eg-data)
- 💻 [GitHub](https://github.com/ernestog27)
- 📧 contact@ernestogonzales.com

---

## License & Usage

This project uses synthetic data created for portfolio demonstration purposes. The dashboard structure, DAX measures, and design patterns are original work and free to use as learning references.

**Attribution appreciated but not required.**

---

## Acknowledgments

Data structure inspired by real-world cybersecurity operations at enterprise utility companies. Dashboard design follows Microsoft Power BI best practices and information design principles from Edward Tufte and Stephen Few.

---

*Last Updated: November 2025*
