# SAP BusinessObjects Web Intelligence 4.2 - Complete AI Knowledge Base

> **Optimized for:** Gemini AI
> **Version:** WebI 4.2
> **Focus:** Law Enforcement & Police Data Analysis
> **Last Updated:** 2025-11-21
> **Document Purpose:** Comprehensive guide to enable AI assistance with WebI query writing, formula creation, and troubleshooting

---

## Document Metadata

```yaml
document_type: knowledge_base
domain: sap_businessobjects_webi_4.2
primary_context: law_enforcement_police_data
target_ai: gemini
token_estimate: 50000
includes:
  - formula_library: 100+ formulas
  - code_examples: 200+ working examples
  - error_solutions: 5 error types
  - design_patterns: 7 patterns
  - use_cases: police_data_focused
```

---

## Table of Contents

### PART 1: QUICK REFERENCE
- [1.1 Formula Cheat Sheet](#11-formula-cheat-sheet)
- [1.2 Error Code Quick Reference](#12-error-code-quick-reference)
- [1.3 When to Use What](#13-when-to-use-what)

### PART 2: CORE CONCEPTS
- [2.1 Variables and Qualification](#21-variables-and-qualification)
- [2.2 Calculation Contexts](#22-calculation-contexts)
- [2.3 Query vs Report Structure](#23-query-vs-report-structure)

### PART 3: FORMULA LIBRARY
- [3.1 Conditional Logic](#31-conditional-logic)
- [3.2 Aggregation Functions](#32-aggregation-functions)
- [3.3 Context Operators](#33-context-operators)
- [3.4 Time & Date Functions](#34-time--date-functions)
- [3.5 String Manipulation](#35-string-manipulation)
- [3.6 Formatting Functions](#36-formatting-functions)
- [3.7 Advanced Functions](#37-advanced-functions)

### PART 4: POLICE DATA PATTERNS
- [4.1 Filtering Multiple Entries per ID](#41-filtering-multiple-entries-per-id)
- [4.2 Getting Latest/Most Recent Record](#42-getting-latestmost-recent-record)
- [4.3 Getting Most Complete Record](#43-getting-most-complete-record)
- [4.4 Getting Last Modified Record](#44-getting-last-modified-record)
- [4.5 Priority-Based Record Selection](#45-priority-based-record-selection)

### PART 5: PROBLEM-SOLUTION MAPPING
- [5.1 "I need to..." Scenarios](#51-i-need-to-scenarios)
- [5.2 Common Police Data Tasks](#52-common-police-data-tasks)

### PART 6: ERROR TROUBLESHOOTING
- [6.1 #MULTIVALUE Error](#61-multivalue-error)
- [6.2 #DIV/0 Error](#62-div0-error)
- [6.3 #COMPUTATION Error](#63-computation-error)
- [6.4 #INCOMPATIBLE Error](#64-incompatible-error)
- [6.5 #DATASYNC Error](#65-datasync-error)

### PART 7: OPTIMIZATION GUIDE
- [7.1 Query-Level Optimization](#71-query-level-optimization)
- [7.2 Formula-Level Optimization](#72-formula-level-optimization)
- [7.3 Report-Level Optimization](#73-report-level-optimization)

### PART 8: DESIGN PATTERNS
- [8.1 Safe Division Pattern](#81-safe-division-pattern)
- [8.2 Percentage of Total Pattern](#82-percentage-of-total-pattern)
- [8.3 Running Total with Reset](#83-running-total-with-reset)
- [8.4 Period-over-Period Comparison](#84-period-over-period-comparison)
- [8.5 Top N Records Pattern](#85-top-n-records-pattern)
- [8.6 Status Classification Pattern](#86-status-classification-pattern)
- [8.7 Data Quality Check Pattern](#87-data-quality-check-pattern)

### PART 9: DECISION TREES
- [9.1 Should I use Dimension, Measure, or Detail?](#91-should-i-use-dimension-measure-or-detail)
- [9.2 Which Context Operator?](#92-which-context-operator)
- [9.3 Query Filter vs Report Filter?](#93-query-filter-vs-report-filter)

### APPENDICES
- [Appendix A: Complete Formula Index (Alphabetical)](#appendix-a-complete-formula-index-alphabetical)
- [Appendix B: Formula Index by Category](#appendix-b-formula-index-by-category)
- [Appendix C: Formula Index by Complexity](#appendix-c-formula-index-by-complexity)
- [Appendix D: WebI Glossary](#appendix-d-webi-glossary)

---

# PART 1: QUICK REFERENCE

## 1.1 Formula Cheat Sheet

### Most Common Formulas

| Category | Formula | Example | Use Case |
|----------|---------|---------|----------|
| **Aggregation** | `Sum([Measure])` | `Sum([Incident Count])` | Total incidents |
| | `Count([Object])` | `Count([Case ID])` | Count records |
| | `Count([Object]; Distinct)` | `Count([Officer ID]; Distinct)` | Unique officers |
| | `Average([Measure])` | `Average([Response Time])` | Mean response time |
| | `Min([Measure])` / `Max([Measure])` | `Max([Priority Level])` | Highest priority |
| **Conditional** | `If [Condition] Then [Value] Else [Value]` | `If [Priority] = "High" Then "Urgent" Else "Standard"` | Status classification |
| **Context** | `[Measure] In ([Dim1]; [Dim2])` | `Max([Date]) In ([Case ID])` | Latest date per case |
| | `[Measure] ForEach([Dim])` | `Count([Events]) ForEach([Officer])` | Events per officer |
| | `[Measure] ForAll([Dim])` | `Sum([Count]) ForAll([District])` | Total across districts |
| | `[Measure] Where ([Condition])` | `Count([Cases]) Where ([Status] = "Open")` | Filtered count |
| **String** | `[String1] + [String2]` | `[First Name] + " " + [Last Name]` | Full name |
| | `Left([String]; N)` | `Left([Case Number]; 4)` | Year prefix |
| | `Right([String]; N)` | `Right([Badge Number]; 3)` | Badge suffix |
| | `Upper([String])` / `Lower([String])` | `Upper([Location])` | Uppercase |
| **Date** | `CurrentDate()` | `CurrentDate()` | Today's date |
| | `Year([Date])` / `Month([Date])` | `Year([Incident Date])` | Extract year |
| | `RelativeDate([Date]; Days)` | `RelativeDate(CurrentDate(); -30)` | 30 days ago |
| | `DaysBetween([Date1]; [Date2])` | `DaysBetween([Call Time]; [Arrival Time])` | Response time in days |
| **Time Series** | `Previous([Measure])` | `Previous([Incident Count])` | Previous period |
| | `RunningSum([Measure])` | `RunningSum([Cases])` | Cumulative total |
| **Advanced** | `NoFilter([Measure])` | `[Count] / NoFilter(Sum([Count]))` | Percentage of unfiltered total |
| | `Rank([Measure])` | `Rank([Arrest Count])` | Ranking |

---

## 1.2 Error Code Quick Reference

| Error Code | Common Cause | Quick Fix |
|------------|--------------|-----------|
| **#MULTIVALUE** | Multiple values returned for single cell | Add dimension to block OR use aggregation: `Max([Field]) In ([ID])` |
| **#DIV/0** | Division by zero | Add check: `If [Denominator] <> 0 Then [Num]/[Denom] Else 0` |
| **#COMPUTATION** | Dimension missing from block | Add dimension to block OR adjust context operators |
| **#INCOMPATIBLE** | Dimensions from incompatible contexts | Merge dimensions OR create detail variables |
| **#DATASYNC** | Unsynchronized data providers | Merge common dimensions between providers |

---

## 1.3 When to Use What

### Variable Qualification Decision

```
Is the result TEXT/CATEGORICAL?
  └─ YES → Use DIMENSION
       Examples: Status flags, categories, classifications

Is the result NUMERIC and needs AGGREGATION?
  └─ YES → Use MEASURE
       Examples: Counts, sums, averages, ratios

Is the result ADDITIONAL INFO that shouldn't aggregate?
  └─ YES → Use DETAIL
       Examples: Officer names, addresses, reference data
```

### Context Operator Decision

```
Do you know EXACTLY which dimensions to use?
  └─ YES → Use IN: [Measure] In ([Dim1]; [Dim2])

Do you need to ADD a dimension not in the block?
  └─ YES → Use ForEach: [Measure] ForEach([Dim])

Do you need to REMOVE a dimension for higher-level aggregation?
  └─ YES → Use ForAll: [Measure] ForAll([Dim])

Do you need to FILTER during calculation?
  └─ YES → Use Where: [Measure] Where ([Status] = "Active")
```

### Filter Decision

```
Is the filter value FIXED and known at report design time?
  └─ YES → Use QUERY FILTER (much faster)

Do users need to change filter dynamically?
  └─ YES → Use PROMPT in query filter

Is this for ad-hoc exploration?
  └─ YES → Use REPORT FILTER (more flexible)
```

---

# PART 2: CORE CONCEPTS

## 2.1 Variables and Qualification

### Understanding Variable Types

WebI variables can be qualified as **Dimension**, **Measure**, or **Detail**. Choosing the correct qualification is critical to avoid errors and ensure correct calculations.

#### Dimension Variables
**Purpose:** Categorical data, grouping, classification

**When to use:**
- Status flags ("Open", "Closed", "Pending")
- Classifications ("High Priority", "Medium Priority", "Low Priority")
- Categories ("Robbery", "Assault", "Theft")
- Any TEXT-based conditional that groups data

**Police Data Example:**
```webi
// Variable: var_Priority_Category
// Qualification: Dimension
If [Priority Level] >= 8 Then "Critical"
ElseIf [Priority Level] >= 5 Then "High"
ElseIf [Priority Level] >= 3 Then "Medium"
Else "Low"
```

**Police Data Example 2:**
```webi
// Variable: var_Case_Status
// Qualification: Dimension
If [Days Since Opened] > 180 Then "Cold Case"
ElseIf [Days Since Opened] > 90 Then "Stale"
ElseIf [Status] = "Active" Then "Active Investigation"
Else "Recent"
```

**Common Error:** Setting a text-based classification as a Measure causes **#MULTIVALUE** errors.

---

#### Measure Variables
**Purpose:** Numeric calculations that need aggregation

**When to use:**
- Counts (incident counts, arrest counts)
- Sums (total response time, total cases)
- Calculations (clearance rate, average response time)
- Ratios and percentages
- Any NUMERIC result that should aggregate automatically

**Police Data Example:**
```webi
// Variable: var_Response_Time_Minutes
// Qualification: Measure
DaysBetween([Call Time]; [Arrival Time]) * 24 * 60
```

**Police Data Example 2:**
```webi
// Variable: var_Clearance_Rate
// Qualification: Measure
If Count([Cases]) <> 0 Then
    (Count([Cases]) Where ([Status] = "Closed")) / Count([Cases]) * 100
Else 0
```

**Police Data Example 3:**
```webi
// Variable: var_Cases_Per_Officer
// Qualification: Measure
If Count([Officer ID]; Distinct) <> 0 Then
    Count([Case ID]) / Count([Officer ID]; Distinct)
Else 0
```

**Key Benefit:** Measures automatically aggregate based on dimensions in the block. This is more efficient than cell-level calculations.

---

#### Detail Variables
**Purpose:** Additional information without aggregation

**When to use:**
- Officer names, badge numbers
- Location descriptions, addresses
- Reference data that should appear as-is
- Converting problematic dimensions to details

**Police Data Example:**
```webi
// Variable: var_Full_Officer_Name
// Qualification: Detail
[Officer First Name] + " " + [Officer Last Name]
```

**Police Data Example 2:**
```webi
// Variable: var_Incident_Address
// Qualification: Detail
[Street Number] + " " + [Street Name] + ", " + [City] + ", " + [State]
```

**When to convert Dimension to Detail:**
- To avoid unwanted grouping/aggregation
- When you have many unique values causing performance issues

---

### Division Safety Pattern

**Always check for zero denominators to avoid #DIV/0 errors.**

**Pattern:**
```webi
If [Denominator] <> 0 Then [Numerator] / [Denominator] Else 0
```

**Police Data Examples:**

```webi
// Variable: var_Case_Closure_Rate
If [Total Cases] <> 0 Then
    [Closed Cases] / [Total Cases] * 100
Else 0
```

```webi
// Variable: var_Avg_Response_Minutes
If [Incident Count] <> 0 Then
    [Total Response Minutes] / [Incident Count]
Else 0
```

```webi
// Variable: var_Arrest_To_Incident_Ratio
If IsNull([Incident Count]) Or [Incident Count] = 0 Then 0
Else [Arrest Count] / [Incident Count]
```

**Note:** Also check for NULL values using `IsNull()` when data quality is uncertain.

---

### Naming Conventions Best Practice

**Use consistent prefixes to distinguish report variables from universe objects:**

| Prefix | Type | Example |
|--------|------|---------|
| `var_` | General variable | `var_TotalIncidents` |
| `dim_` | Dimension variable | `dim_PriorityLevel` |
| `msr_` | Measure variable | `msr_AvgResponseTime` |

**Benefits:**
- Immediately identifies report-level variables
- Prevents confusion with universe objects
- Makes formulas easier to read and debug
- Easier maintenance

**Police Data Examples:**
```webi
var_CaseAgeInDays
dim_IncidentCategory
msr_OfficerWorkload
var_PreviousMonthCases
dim_DistrictClassification
```

---

## 2.2 Calculation Contexts

**Calculation context** determines which dimensions are used to aggregate measures. Understanding contexts is fundamental to WebI formula writing.

### Default Context

**Concept:** WebI automatically determines aggregation context based on dimensions displayed in the block.

**Example Block:**

| District | Month | Incident Count |
|----------|-------|----------------|
| North    | Jan   | 150            |
| North    | Feb   | 175            |
| South    | Jan   | 200            |
| South    | Feb   | 210            |

**Default context for Incident Count:** `In ([District]; [Month])`

Each cell shows incidents for that specific District-Month combination.

---

### Context Operators (IN, ForEach, ForAll, Where)

#### IN Operator

**Purpose:** Explicitly specify which dimensions to use in calculation

**Syntax:** `[Measure] In ([Dimension1]; [Dimension2])`

**When to use:** When you know exactly which dimensions should affect the calculation

**Police Data Example 1:**
```webi
// Get latest incident date per case, regardless of other dimensions in block
Max([Incident Date]) In ([Case ID])
```

**Scenario:** Report shows Case ID, Officer ID, District, and Incident Date. You want the latest incident date for each case, ignoring officer and district.

**Police Data Example 2:**
```webi
// Total incidents per district per year, ignoring month
Sum([Incident Count]) In ([District]; [Year])
```

**Result:** Shows annual district totals even if Month is in the block.

**Police Data Example 3:**
```webi
// Highest priority per case
Max([Priority Level]) In ([Case ID])
```

---

#### ForEach Operator

**Purpose:** Add dimensions to the default context

**Syntax:** `[Measure] ForEach ([AdditionalDimension])`

**When to use:** When a dimension exists in the query but isn't displayed in the block

**Police Data Example 1:**
```webi
// Count incidents at district level even though district isn't shown
Count([Incident ID]) ForEach([District])
```

**Scenario:** Report shows only Year and Month. District is in query but not displayed. You want incident counts calculated per district, then aggregated.

**Police Data Example 2:**
```webi
// Officer workload calculation including hidden shift dimension
Sum([Case Count]) ForEach([Officer ID]; [Shift])
```

**Police Data Example 3:**
```webi
// Get maximum priority including case type (in query but not shown)
Max([Priority]) ForEach([Case Type])
```

---

#### ForAll Operator

**Purpose:** Remove dimensions from the default context for higher-level aggregation

**Syntax:** `[Measure] ForAll ([DimensionToRemove])`

**When to use:** When you need totals at a higher aggregation level than what's displayed

**Police Data Example:**

**Block contains:** District, Month, Incident Count

```webi
// Variable: var_District_Annual_Total
// Shows district annual total on every row
Sum([Incident Count]) ForAll([Month])
```

**Result:**

| District | Month | Incident Count | var_District_Annual_Total |
|----------|-------|----------------|---------------------------|
| North    | Jan   | 150            | 900                       |
| North    | Feb   | 175            | 900                       |
| North    | Mar   | 200            | 900                       |
| South    | Jan   | 200            | 1200                      |
| South    | Feb   | 210            | 1200                      |

The ForAll removes Month from aggregation, giving annual district totals.

**Police Data Example 2:**
```webi
// Percentage of district total
([Incident Count] / Sum([Incident Count]) ForAll([Month])) * 100
```

This shows each month as a percentage of the district's annual total.

---

#### Where Operator

**Purpose:** Filter data during calculation

**Syntax:** `[Measure] Where ([Condition] = "ConstantValue")`

**When to use:** To calculate filtered aggregates without changing the report filter

**IMPORTANT LIMITATION:** The right side of the comparison must be a **constant value**, not a dynamic field.

**Police Data Examples:**

```webi
// Count only high-priority cases
Count([Case ID]) Where ([Priority] = "High")
```

```webi
// Sum incidents for violent crimes only
Sum([Incident Count]) Where ([Crime Category] = "Violent")
```

```webi
// Count cases opened this year
Count([Case ID]) Where (Year([Open Date]) = Year(CurrentDate()))
```

```webi
// Average response time for emergency calls
Average([Response Minutes]) Where ([Call Type] = "Emergency")
```

```webi
// Multiple conditions
Sum([Incidents]) Where ([District] = "North" And [Priority Level] > 5)
```

**You CANNOT do this:**
```webi
// ❌ WRONG - Right side must be constant
Count([Cases]) Where ([District] = [Officer Home District])
```

---

### Output Context Keywords

**Purpose:** Control the scope of calculations across different report levels

**Keywords:** `Report`, `Section`, `Block`, `Body`

| Keyword | Scope | Example Use Case |
|---------|-------|------------------|
| `Report` | Entire document | Grand totals across all pages |
| `Section` | Current section | Section-level subtotals |
| `Block` | Current table | Table-level calculations |
| `Body` | Individual cells | Cell-level calculations (default) |

**Police Data Example 1 - Percentage of Total:**
```webi
// Variable: var_Percent_Of_Total_Cases
([Case Count] / Sum([Case Count]) In Report) * 100
```

Shows each district/unit as a percentage of citywide total.

**Police Data Example 2 - Section Totals:**

If report is sectioned by District:
```webi
// Variable: var_District_Total
Sum([Incident Count]) In Section
```

Shows total for current district section.

**Police Data Example 3 - Combined:**
```webi
// Percentage of district total within citywide report
([Incidents] / Sum([Incidents]) In Section) * 100
```

**Practical Example:**

| District | Month | Incidents | var_Pct_District | var_Pct_Citywide |
|----------|-------|-----------|------------------|------------------|
| North    | Jan   | 50        | 33.3%            | 10.0%            |
| North    | Feb   | 60        | 40.0%            | 12.0%            |
| North    | Mar   | 40        | 26.7%            | 8.0%             |
| **North Total** | | **150** | **100%**     | **30.0%**        |

Where:
- `var_Pct_District = ([Incidents] / Sum([Incidents]) In Section) * 100`
- `var_Pct_Citywide = ([Incidents] / Sum([Incidents]) In Report) * 100`

---

## 2.3 Query vs Report Structure

### Query Filters vs Report Filters

**Critical Performance Distinction:**

| Aspect | Query Filter | Report Filter |
|--------|--------------|---------------|
| **Applied** | At database level | In WebI after data retrieval |
| **Performance** | Fast - limits data transferred | Slow - retrieves all data first |
| **Speed Difference** | 10-100x faster for large datasets | Slower |
| **When to Use** | Fixed requirements, large datasets | Interactive analysis, ad-hoc filtering |
| **Flexibility** | Less flexible after report creation | Highly flexible |
| **Best Practice** | Use whenever possible | Use for user interactivity |

**Police Data Examples:**

**Use QUERY FILTER when:**
- Filtering to current year's cases
- Limiting to specific districts (known at design time)
- Excluding test/invalid records
- Large datasets (>100K records)

**Use REPORT FILTER when:**
- Users need to explore different districts interactively
- Ad-hoc analysis scenarios
- Small to medium datasets

**Query Filter Example:**
```
Object: [Incident Date]
Operator: Between
Values: 01/01/2024 and 31/12/2024
```

**With Prompt:**
```
Object: [District]
Operator: Equal to
Value: @Prompt('Select District:', 'A', 'District\\District Name', mono, constrained)
```

---

### Prompts Best Practices

**Prompt Syntax:**
```
@Prompt(description, answer_type, class\object, multiplicity, constrained_type)
```

**Parameters:**
- `description`: Text shown to user
- `answer_type`: 'A' (alphanumeric), 'N' (numeric), 'D' (date)
- `class\object`: Universe path (use \\\\ for single \\)
- `multiplicity`: mono (single value), multi (multiple values)
- `constrained_type`: constrained (from list), free (type any value)

**Police Data Examples:**

```webi
// Single district selection
@Prompt('Select District:', 'A', 'Geography\\District', mono, constrained)
```

```webi
// Multiple crime types
@Prompt('Select Crime Types:', 'A', 'Incident\\Crime Type', multi, constrained)
```

```webi
// Date range
@Prompt('Enter Start Date:', 'D', '', mono, free)
```

```webi
// Officer badge number (free entry)
@Prompt('Enter Badge Number:', 'N', '', mono, free)
```

**Advanced Prompt with Options:**
```webi
@Prompt('Select Status:', 'A', 'Case\\Status', multi, constrained, Not_Persistent, , User:1)
```
- `Not_Persistent`: Doesn't remember last value (prevents stale filters)
- `User:1`: Defines prompt order (shows first)

---

### Scope of Analysis

**Purpose:** Controls drilling capability and data pre-fetching

**Options:**

| Level | Drilling | Initial Query | Use Case |
|-------|----------|---------------|----------|
| **None** | Disabled | Fastest | Static reports, no drilling needed |
| **One Level** | 1 level down | Fast | Limited drilling (e.g., District → Station) |
| **Two Levels** | 2 levels down | Medium | Moderate drilling |
| **Three Levels** | 3 levels down | Slower | Extensive drilling |
| **Custom** | Specific objects | Variable | Controlled drill paths |

**Trade-off:**
- **With scope:** Slower initial query, fast drilling (data already loaded)
- **Without scope:** Fast initial query, each drill hits database (query drill)

**Police Data Recommendation:**
- **Reports for executives:** One level (District → Station)
- **Analyst reports:** Two-three levels (District → Station → Beat → Officer)
- **Static dashboards:** None (no drilling)

---

# PART 3: FORMULA LIBRARY

This section contains 100+ formulas organized by category, all with police data examples.

## 3.1 Conditional Logic

Conditional logic allows you to make decisions in formulas based on data values.

### Simple If Statement

**Syntax:** `If [Condition] Then [Value] Else [Value]`

**Complexity:** Beginner

**Police Examples:**

```webi
// Variable: var_Priority_Flag
If [Priority Level] >= 7 Then "Urgent" Else "Standard"
```

```webi
// Variable: var_Case_Age_Flag
If [Days Open] > 30 Then "Overdue" Else "Current"
```

```webi
// Variable: var_Response_Status
If [Response Minutes] <= 10 Then "On Target" Else "Delayed"
```

```webi
// Variable: var_Case_Assigned
If [Officer ID] <> "" Then "Assigned" Else "Unassigned"
```

---

### Nested If Statements

**Syntax:** `If [Cond1] Then [Val1] ElseIf [Cond2] Then [Val2] Else [Val3]`

**Complexity:** Intermediate

**Police Examples:**

```webi
// Variable: var_Incident_Severity
If [Injury Count] > 0 Then "Critical"
ElseIf [Property Damage] > 50000 Then "Serious"
ElseIf [Weapon Involved] = "Yes" Then "Elevated"
Else "Standard"
```

```webi
// Variable: var_Case_Priority_Category
If [Priority Level] >= 8 Then "Critical"
ElseIf [Priority Level] >= 6 Then "High"
ElseIf [Priority Level] >= 4 Then "Medium"
ElseIf [Priority Level] >= 2 Then "Low"
Else "Minimal"
```

```webi
// Variable: var_Officer_Workload_Status
If [Active Cases] > 20 Then "Overloaded"
ElseIf [Active Cases] > 15 Then "High Load"
ElseIf [Active Cases] > 10 Then "Normal Load"
ElseIf [Active Cases] > 5 Then "Light Load"
Else "Minimal Load"
```

```webi
// Variable: var_Evidence_Quality_Rating
If [Evidence Items] >= 10 And [Witness Count] >= 3 Then "Excellent"
ElseIf [Evidence Items] >= 5 And [Witness Count] >= 2 Then "Good"
ElseIf [Evidence Items] >= 2 Or [Witness Count] >= 1 Then "Fair"
Else "Poor"
```

---

### And Logic

**Syntax:** `If [Condition1] And [Condition2] Then [Value] Else [Value]`

**Complexity:** Intermediate

**Use:** All conditions must be true

**Police Examples:**

```webi
// Variable: var_High_Risk_Case
If [Priority Level] >= 8 And [Weapon Involved] = "Yes" Then "High Risk" Else "Standard"
```

```webi
// Variable: var_Cold_Case_Priority
If [Days Open] > 365 And [Evidence Items] > 0 And [Witness Count] > 0 Then "Workable Cold Case" Else "Cold Case - Low Evidence"
```

```webi
// Variable: var_Urgent_Investigation
If [Crime Category] = "Violent" And [Days Open] < 7 And [Leads Count] > 0 Then "Priority Investigation" Else "Standard"
```

```webi
// Variable: var_Case_Ready_For_Review
If [Status] = "Complete" And [Evidence Logged] = "Yes" And [Report Filed] = "Yes" Then "Ready" Else "Pending"
```

---

### Or Logic

**Syntax:** `If [Condition1] Or [Condition2] Then [Value] Else [Value]`

**Complexity:** Intermediate

**Use:** Any condition can be true

**Police Examples:**

```webi
// Variable: var_Needs_Attention
If [Priority Level] >= 8 Or [Days Open] > 90 Or [Victim Calls] > 5 Then "Needs Attention" Else "Standard"
```

```webi
// Variable: var_Special_Handling
If [Media Involved] = "Yes" Or [VIP Involved] = "Yes" Or [Political Sensitivity] = "High" Then "Special Handling" Else "Normal"
```

```webi
// Variable: var_Escalation_Required
If [Complaints Filed] > 0 Or [Supervisor Flagged] = "Yes" Or [Legal Hold] = "Yes" Then "Escalate" Else "Normal Processing"
```

```webi
// Variable: var_Requires_Specialist
If [Crime Type] = "Cybercrime" Or [Crime Type] = "Fraud" Or [Crime Type] = "Forensics Required" Then "Specialist Needed" Else "General Detective"
```

---

## 3.2 Aggregation Functions

Aggregation functions perform calculations across multiple rows.

### Sum

**Syntax:** `Sum([Measure])`

**Complexity:** Beginner

**Use:** Total of all values

**Police Examples:**

```webi
// Total incidents
Sum([Incident Count])
```

```webi
// Total arrests across all districts
Sum([Arrest Count])
```

```webi
// Total response time in minutes
Sum([Response Minutes])
```

```webi
// Total property damage value
Sum([Property Damage Amount])
```

**With Context:**

```webi
// Total incidents per district (regardless of month in block)
Sum([Incident Count]) In ([District])
```

```webi
// Total arrests per officer
Sum([Arrest Count]) In ([Officer ID])
```

---

### Count

**Syntax:** `Count([Object])`

**Complexity:** Beginner

**Use:** Number of non-null records

**Police Examples:**

```webi
// Count of cases
Count([Case ID])
```

```webi
// Count of incidents
Count([Incident ID])
```

```webi
// Count of officers
Count([Officer ID])
```

```webi
// Count of evidence items
Count([Evidence ID])
```

**With Filter:**

```webi
// Count of open cases only
Count([Case ID]) Where ([Status] = "Open")
```

```webi
// Count of violent crimes
Count([Incident ID]) Where ([Crime Category] = "Violent")
```

---

### Count Distinct

**Syntax:** `Count([Object]; Distinct)`

**Complexity:** Intermediate

**Use:** Count unique values only

**Police Examples:**

```webi
// Unique officers assigned
Count([Officer ID]; Distinct)
```

```webi
// Unique case types
Count([Case Type]; Distinct)
```

```webi
// Unique suspects involved
Count([Suspect ID]; Distinct)
```

```webi
// Unique locations with incidents
Count([Location ID]; Distinct)
```

```webi
// Unique victims across cases
Count([Victim ID]; Distinct) In ([Case ID])
```

---

### Average

**Syntax:** `Average([Measure])`

**Complexity:** Beginner

**Use:** Mean value

**Police Examples:**

```webi
// Average response time in minutes
Average([Response Minutes])
```

```webi
// Average case resolution days
Average([Days To Close])
```

```webi
// Average priority level
Average([Priority Level])
```

```webi
// Average incidents per day
Average([Incident Count]) In ([Date])
```

**With Context:**

```webi
// Average response time per district
Average([Response Minutes]) In ([District])
```

---

### Min and Max

**Syntax:** `Min([Measure])` / `Max([Measure])`

**Complexity:** Beginner

**Use:** Minimum or maximum value

**Police Examples:**

```webi
// Fastest response time
Min([Response Minutes])
```

```webi
// Slowest response time
Max([Response Minutes])
```

```webi
// Earliest incident date
Min([Incident Date])
```

```webi
// Latest incident date
Max([Incident Date])
```

```webi
// Highest priority level
Max([Priority Level]) In ([Case ID])
```

```webi
// Oldest open case date
Min([Open Date]) Where ([Status] = "Open")
```

---

### Conditional Aggregations

**Syntax:** `Sum/Count/Average([Measure]) Where ([Condition])`

**Complexity:** Intermediate

**Use:** Aggregate with filter

**Police Examples:**

```webi
// Count of high-priority cases
Count([Case ID]) Where ([Priority Level] >= 7)
```

```webi
// Total violent crime incidents
Sum([Incident Count]) Where ([Crime Category] = "Violent")
```

```webi
// Average response time for emergency calls
Average([Response Minutes]) Where ([Call Type] = "Emergency")
```

```webi
// Count of cases closed this year
Count([Case ID]) Where (Year([Close Date]) = Year(CurrentDate()))
```

```webi
// Sum of property damage for burglaries
Sum([Property Damage]) Where ([Crime Type] = "Burglary")
```

```webi
// Count of cases assigned to District North
Count([Case ID]) Where ([District] = "North")
```

---

## 3.3 Context Operators

Context operators control which dimensions are used in calculations. (See Part 2.2 for detailed explanations)

### IN Operator

**Syntax:** `[Measure] In ([Dim1]; [Dim2])`

**Complexity:** Intermediate

**Police Examples:**

```webi
// Latest incident date per case
Max([Incident Date]) In ([Case ID])
```

```webi
// Total incidents per district and year
Sum([Incident Count]) In ([District]; [Year])
```

```webi
// Highest priority per officer
Max([Priority Level]) In ([Officer ID])
```

```webi
// Count of cases per district
Count([Case ID]) In ([District])
```

```webi
// Most recent call time per location
Max([Call Time]) In ([Location ID])
```

---

### ForEach Operator

**Syntax:** `[Measure] ForEach([AdditionalDim])`

**Complexity:** Intermediate

**Police Examples:**

```webi
// Count incidents including hidden beat dimension
Count([Incident ID]) ForEach([Beat])
```

```webi
// Sum cases including officer shift (not shown in block)
Sum([Case Count]) ForEach([Shift])
```

```webi
// Average response including hidden call type
Average([Response Minutes]) ForEach([Call Type])
```

---

### ForAll Operator

**Syntax:** `[Measure] ForAll([DimensionToRemove])`

**Complexity:** Intermediate

**Police Examples:**

```webi
// District annual total (removes month)
Sum([Incident Count]) ForAll([Month])
```

```webi
// Citywide total (removes district)
Sum([Case Count]) ForAll([District])
```

```webi
// Percentage of district total
([Incidents] / Sum([Incidents]) ForAll([Month])) * 100
```

---

### Where Operator

**Syntax:** `[Measure] Where ([Condition] = "Value")`

**Complexity:** Intermediate

**Police Examples:**

```webi
// Count violent crimes only
Count([Incident ID]) Where ([Category] = "Violent")
```

```webi
// Sum arrests for drug offenses
Sum([Arrest Count]) Where ([Offense Type] = "Drug")
```

```webi
// Average response for priority calls
Average([Response Minutes]) Where ([Priority Level] >= 7)
```

```webi
// Count cases opened this year
Count([Case ID]) Where (Year([Open Date]) = Year(CurrentDate()))
```

---

## 3.4 Time & Date Functions

Date and time functions are critical for police data analysis.

### CurrentDate

**Syntax:** `CurrentDate()`

**Complexity:** Beginner

**Returns:** Today's date

**Police Examples:**

```webi
// Calculate case age in days
DaysBetween([Open Date]; CurrentDate())
```

```webi
// Flag cases older than 30 days
If DaysBetween([Open Date]; CurrentDate()) > 30 Then "Overdue" Else "Current"
```

```webi
// Display report run date
"Report Generated: " + FormatDate(CurrentDate(); "dd/MM/yyyy")
```

---

### Year, Month, Day

**Syntax:** `Year([Date])` / `Month([Date])` / `Day([Date])`

**Complexity:** Beginner

**Returns:** Year (2024), Month (1-12), Day (1-31)

**Police Examples:**

```webi
// Extract year from incident date
Year([Incident Date])
```

```webi
// Extract month number
Month([Call Date])
```

```webi
// Extract day of month
Day([Arrest Date])
```

```webi
// Filter current year incidents
If Year([Incident Date]) = Year(CurrentDate()) Then [Incident Count] Else 0
```

```webi
// Month name variable
If Month([Date]) = 1 Then "January"
ElseIf Month([Date]) = 2 Then "February"
// ... etc
```

---

### DayName

**Syntax:** `DayName([Date])`

**Complexity:** Intermediate

**Returns:** "Monday", "Tuesday", etc.

**Police Examples:**

```webi
// Day of week for incident
DayName([Incident Date])
```

```webi
// Identify weekend incidents
If DayName([Incident Date]) In ("Saturday"; "Sunday") Then "Weekend" Else "Weekday"
```

```webi
// Weekend incident flag
If DayName([Call Date]) = "Saturday" Or DayName([Call Date]) = "Sunday" Then "Yes" Else "No"
```

---

### RelativeDate

**Syntax:** `RelativeDate([Date]; DaysOffset)`

**Complexity:** Intermediate

**Use:** Add or subtract days from a date

**Police Examples:**

```webi
// 30 days ago
RelativeDate(CurrentDate(); -30)
```

```webi
// 7 days from incident
RelativeDate([Incident Date]; 7)
```

```webi
// Cases opened in last 30 days
If [Open Date] >= RelativeDate(CurrentDate(); -30) Then "Recent" Else "Older"
```

```webi
// Response deadline (incident date + 2 days)
RelativeDate([Incident Date]; 2)
```

**WebI 4.2 Period Types:**

```webi
// Last month
RelativeDate(CurrentDate(); -1; MonthPeriod)
```

```webi
// Last quarter
RelativeDate(CurrentDate(); -1; QuarterPeriod)
```

```webi
// Last year
RelativeDate(CurrentDate(); -1; YearPeriod)
```

---

### DaysBetween

**Syntax:** `DaysBetween([StartDate]; [EndDate])`

**Complexity:** Intermediate

**Returns:** Number of days between dates

**Police Examples:**

```webi
// Case age in days
DaysBetween([Open Date]; CurrentDate())
```

```webi
// Days to close case
DaysBetween([Open Date]; [Close Date])
```

```webi
// Response time in days
DaysBetween([Call Date]; [Arrival Date])
```

```webi
// Days since last activity
DaysBetween([Last Activity Date]; CurrentDate())
```

**Convert to hours/minutes:**

```webi
// Response time in hours
DaysBetween([Call Time]; [Arrival Time]) * 24
```

```webi
// Response time in minutes
DaysBetween([Call Time]; [Arrival Time]) * 24 * 60
```

---

### FormatDate

**Syntax:** `FormatDate([Date]; "FormatString")`

**Complexity:** Beginner

**Use:** Custom date display format

**Police Examples:**

```webi
// Standard date format
FormatDate([Incident Date]; "dd/MM/yyyy")
// Output: 25/12/2024
```

```webi
// Month and year only
FormatDate([Report Date]; "MMMM yyyy")
// Output: December 2024
```

```webi
// Short format
FormatDate([Call Date]; "dd-MMM-yy")
// Output: 25-Dec-24
```

```webi
// Full date with day name
FormatDate([Incident Date]; "dddd, dd MMMM yyyy")
// Output: Monday, 25 December 2024
```

```webi
// ISO format
FormatDate([Date]; "yyyy-MM-dd")
// Output: 2024-12-25
```

---

### ToDate

**Syntax:** `ToDate([StringValue]; "FormatString")`

**Complexity:** Intermediate

**Use:** Convert string to date

**Police Examples:**

```webi
// Parse date from text field
ToDate([Date Text Field]; "yyyy-MM-dd")
```

```webi
// Convert text to date
ToDate("2024-01-15"; "yyyy-MM-dd")
```

```webi
// Parse date with different format
ToDate([Legacy Date Field]; "dd/MM/yyyy")
```

---

### Year-to-Date (YTD) Calculations

**Complexity:** Advanced

**Police Examples:**

```webi
// YTD incidents
Sum([Incident Count]) Where (
    Year([Incident Date]) = Year(CurrentDate())
    And Month([Incident Date]) <= Month(CurrentDate())
)
```

```webi
// YTD arrests
Sum([Arrest Count]) Where (
    [Arrest Date] >= ToDate("01/01/" + FormatNumber(Year(CurrentDate()); "0000"); "dd/MM/yyyy")
    And [Arrest Date] <= CurrentDate()
)
```

```webi
// Cases opened YTD
Count([Case ID]) Where (
    Year([Open Date]) = Year(CurrentDate())
    And [Open Date] <= CurrentDate()
)
```

---

### Quarter Calculations

**Complexity:** Intermediate

**Police Examples:**

```webi
// Current quarter
"Q" + FormatNumber(Ceil(Month(CurrentDate()) / 3); "0")
// Output: Q4
```

```webi
// Quarter from date
"Q" + FormatNumber(Ceil(Month([Incident Date]) / 3); "0")
```

```webi
// Quarter classification
If Month([Date]) <= 3 Then "Q1"
ElseIf Month([Date]) <= 6 Then "Q2"
ElseIf Month([Date]) <= 9 Then "Q3"
Else "Q4"
```

---

### Working Days Calculation

**Complexity:** Advanced

**Police Examples:**

```webi
// Previous working day
If DayName(CurrentDate()) = "Monday" Then RelativeDate(CurrentDate(); -3)
ElseIf DayName(CurrentDate()) = "Sunday" Then RelativeDate(CurrentDate(); -2)
Else RelativeDate(CurrentDate(); -1)
```

```webi
// Is working day?
If DayName([Date]) <> "Saturday" And DayName([Date]) <> "Sunday" Then "Yes" Else "No"
```

---

## 3.5 String Manipulation

String functions for text processing and parsing.

### Concatenation

**Syntax:** `[String1] + [String2]`

**Complexity:** Beginner

**Police Examples:**

```webi
// Full name
[First Name] + " " + [Last Name]
```

```webi
// Full officer name with badge
[Officer Name] + " (Badge: " + [Badge Number] + ")"
```

```webi
// Complete address
[Street Number] + " " + [Street Name] + ", " + [City] + ", " + [State] + " " + [Zip Code]
```

```webi
// Case reference
[Case Number] + " - " + [Case Type]
```

```webi
// Incident summary
[Crime Type] + " at " + [Location] + " on " + FormatDate([Incident Date]; "dd/MM/yyyy")
```

---

### Left Function

**Syntax:** `Left([String]; N)`

**Complexity:** Beginner

**Use:** Extract first N characters

**Police Examples:**

```webi
// Year from case number (first 4 characters)
Left([Case Number]; 4)
```

```webi
// District code (first 2 characters of location code)
Left([Location Code]; 2)
```

```webi
// Incident category prefix
Left([Incident ID]; 3)
```

```webi
// Badge division (first 3 digits)
Left([Badge Number]; 3)
```

---

### Right Function

**Syntax:** `Right([String]; N)`

**Complexity:** Beginner

**Use:** Extract last N characters

**Police Examples:**

```webi
// Sequential number (last 4 digits of case number)
Right([Case Number]; 4)
```

```webi
// Badge suffix
Right([Badge Number]; 3)
```

```webi
// Last 4 of phone number
Right([Phone Number]; 4)
```

```webi
// Last 4 of SSN
"XXX-XX-" + Right([SSN]; 4)
```

---

### Substr / Mid Function

**Syntax:** `Substr([String]; StartPosition; Length)`

**Complexity:** Intermediate

**Use:** Extract substring from specific position

**Police Examples:**

```webi
// Extract month from case number (positions 5-6)
Substr([Case Number]; 5; 2)
```

```webi
// Extract district code (positions 3-5)
Substr([Location ID]; 3; 3)
```

```webi
// Extract beat number
Substr([Beat Code]; 4; 2)
```

```webi
// Extract middle initial
Substr([Full Name]; Pos([Full Name]; " ") + 1; 1)
```

---

### Upper, Lower, InitCap

**Syntax:** `Upper([String])` / `Lower([String])` / `InitCap([String])`

**Complexity:** Beginner

**Police Examples:**

```webi
// Uppercase location for consistency
Upper([Location Name])
// Output: "MAIN STREET"
```

```webi
// Lowercase email
Lower([Email Address])
// Output: "officer@police.gov"
```

```webi
// Proper case for names
InitCap([Suspect Name])
// Output: "John Smith"
```

```webi
// Standardize district names
Upper([District])
```

---

### Replace Function

**Syntax:** `Replace([String]; "FindText"; "ReplaceText")`

**Complexity:** Beginner

**Use:** Find and replace text

**Police Examples:**

```webi
// Remove hyphens from phone number
Replace([Phone Number]; "-"; "")
// Input: "555-123-4567"
// Output: "5551234567"
```

```webi
// Remove spaces from badge number
Replace([Badge Number]; " "; "")
```

```webi
// Standardize abbreviations
Replace([Location]; "St."; "Street")
```

```webi
// Clean case numbers
Replace([Case Number]; "/"; "-")
```

---

### Pos Function (Position)

**Syntax:** `Pos([String]; "SearchText")`

**Complexity:** Intermediate

**Returns:** Position of text (0 if not found)

**Police Examples:**

```webi
// Find position of @ in email
Pos([Email]; "@")
```

```webi
// Find position of space in name
Pos([Full Name]; " ")
```

```webi
// Check if case number contains district code
Pos([Case Number]; "N01")
```

```webi
// Extract domain from email
Right([Email]; Length([Email]) - Pos([Email]; "@"))
```

---

### Length Function

**Syntax:** `Length([String])`

**Complexity:** Beginner

**Returns:** Number of characters

**Police Examples:**

```webi
// Validate badge number length
If Length([Badge Number]) = 6 Then "Valid" Else "Invalid"
```

```webi
// Check case number format
If Length([Case Number]) = 10 Then "Standard Format" Else "Non-Standard"
```

```webi
// Count characters in notes
Length([Case Notes])
```

---

### Complex String Examples

**Police Examples:**

```webi
// Extract first name from full name
Left([Full Name]; Pos([Full Name]; " ") - 1)
```

```webi
// Extract last name from full name
Right([Full Name]; Length([Full Name]) - Pos([Full Name]; " "))
```

```webi
// Mask SSN
Left([SSN]; 3) + "-XX-" + Right([SSN]; 4)
// Output: "123-XX-7890"
```

```webi
// Extract email username
Left([Email]; Pos([Email]; "@") - 1)
```

```webi
// Build case reference
"Case #" + [Case Number] + " (" + [Status] + ")"
```

---

## 3.6 Formatting Functions

Functions to control display formatting.

### FormatNumber

**Syntax:** `FormatNumber([Number]; "FormatString")`

**Complexity:** Beginner

**Police Examples:**

```webi
// Format incident count with thousands separator
FormatNumber([Incident Count]; "#,##0")
// Output: "1,234"
```

```webi
// Format response time with decimals
FormatNumber([Response Minutes]; "#,##0.00")
// Output: "12.50"
```

```webi
// Format clearance rate as percentage
FormatNumber([Clearance Rate] * 100; "0.0") + "%"
// Output: "85.5%"
```

```webi
// Format property damage as currency
"$" + FormatNumber([Property Damage]; "#,##0.00")
// Output: "$12,500.00"
```

```webi
// Pad case number with leading zeros
FormatNumber([Case Sequential]; "00000")
// Output: "00123"
```

---

### ToNumber

**Syntax:** `ToNumber([String])`

**Complexity:** Beginner

**Use:** Convert string to number

**Police Examples:**

```webi
// Convert text field to number
ToNumber([Text Case Count])
```

```webi
// Parse numeric badge number from text
ToNumber([Badge Text])
```

```webi
// Convert year string to number
ToNumber(Left([Case Number]; 4))
```

---

### Type Conversion

**Police Examples:**

```webi
// Number to string (for concatenation)
"Case " + ("" + [Case Number])
```

```webi
// Ensure numeric calculation
ToNumber([Text Field 1]) + ToNumber([Text Field 2])
```

---

## 3.7 Advanced Functions

Complex functions for specialized scenarios.

### NoFilter Function

**Syntax:** `NoFilter([Measure])` / `NoFilter([Measure]; All)` / `NoFilter([Measure]; Drill)`

**Complexity:** Advanced

**Use:** Ignore report/block filters to calculate on full dataset

**Police Examples:**

```webi
// Percentage of total (ignoring filters)
([Case Count] / NoFilter(Sum([Case Count]))) * 100
```

**Scenario:** Report is filtered to District North. This shows North's percentage of citywide total (unfiltered).

```webi
// Comparison to citywide average
[Incident Rate] / NoFilter(Average([Incident Rate]))
```

```webi
// Rank against all districts (ignoring section filters)
Rank([Arrest Count]; NoFilter)
```

**NoFilter Options:**

| Option | Ignores |
|--------|---------|
| (default) | Report and block filters |
| All | All filters including drill |
| Drill | Report and drill filters |

---

### Rank Function

**Syntax:** `Rank([Measure])` / `Rank([Measure]) In ([Dimension])`

**Complexity:** Intermediate

**Use:** Assign ranking

**Police Examples:**

```webi
// Overall rank by incident count
Rank([Incident Count])
```

```webi
// Rank within district
Rank([Arrest Count]) In ([District])
```

```webi
// Rank officers by case clearance
Rank([Clearance Rate])
```

```webi
// Top 10 flag
If Rank([Crime Count]) <= 10 Then "Top 10" Else "Other"
```

```webi
// Rank stations by response time (ascending = better)
Rank([Avg Response Minutes])
```

---

### IsNull Function

**Syntax:** `IsNull([Object])`

**Complexity:** Intermediate

**Use:** Check for null values

**Police Examples:**

```webi
// Handle null response times
If IsNull([Response Minutes]) Then 0 Else [Response Minutes]
```

```webi
// Check for missing officer assignment
If IsNull([Officer ID]) Then "Unassigned" Else [Officer Name]
```

```webi
// Safe division with null check
If IsNull([Total Cases]) Or [Total Cases] = 0 Then 0
Else [Closed Cases] / [Total Cases]
```

```webi
// Flag incomplete records
If IsNull([Case Type]) Or IsNull([Priority]) Then "Incomplete" Else "Complete"
```

---

### IsError Function

**Syntax:** `IsError([Object])`

**Complexity:** Advanced

**Use:** Check for calculation errors

**Police Examples:**

```webi
// Handle calculation errors gracefully
If IsError([Complex Calculation]) Then 0 Else [Complex Calculation]
```

```webi
// Error flag
If IsError([Clearance Rate]) Then "Error in Calculation" Else FormatNumber([Clearance Rate]; "0.0%")
```

```webi
// Suppress errors in reports
If IsError([Measure]) Then "" Else "" + [Measure]
```

---

### Previous Function

**Syntax:** `Previous([Measure])` / `Previous([Measure]; [ResetDimension])`

**Complexity:** Intermediate

**Use:** Get previous period value

**Requires:** Proper sort order

**Police Examples:**

```webi
// Previous month incidents
Previous([Incident Count])
```

```webi
// Month-over-month change
[Incident Count] - Previous([Incident Count])
```

```webi
// Month-over-month percentage change
If IsNull(Previous([Incidents])) Or Previous([Incidents]) = 0 Then 0
Else (([Incidents] - Previous([Incidents])) / Previous([Incidents])) * 100
```

```webi
// Previous year incidents (with reset by year)
Previous([Incident Count]; [Year])
```

**Important:** Sort dimension properly for Previous() to work correctly!

---

### RunningSum Function

**Syntax:** `RunningSum([Measure])` / `RunningSum([Measure]; [ResetDimension])`

**Complexity:** Intermediate

**Use:** Cumulative total

**Police Examples:**

```webi
// Cumulative incidents year-to-date
RunningSum([Incident Count])
```

```webi
// Cumulative arrests with reset by year
RunningSum([Arrest Count]; [Year])
```

```webi
// Cumulative cases by officer (reset per officer)
RunningSum([Case Count]; [Officer ID])
```

```webi
// Running total of property damage
RunningSum([Property Damage Amount])
```

---

### Hyperlinks and OpenDocument

**Complexity:** Advanced

**Use:** Link to other reports or documents

**Police Examples:**

```webi
// Link to case detail report
="<a href='http://server:8080/BOE/OpenDocument/opendoc/openDocument.jsp?sType=wid&sDocName=Case_Detail_Report&lsSCase_ID:='" + URLEncode([Case ID]) + "'>" + [Case Number] + "</a>"
```

```webi
// Link to officer detail page
="<a href='http://server:8080/BOE/OpenDocument/opendoc/openDocument.jsp?sType=wid&sDocName=Officer_Report&lsSOfficer_ID:='" + URLEncode([Officer ID]) + "'>View Officer Details</a>"
```

```webi
// Link to district dashboard
="<a href='http://server:8080/BOE/OpenDocument/opendoc/openDocument.jsp?sType=wid&sDocName=District_Dashboard&lsSDistrict:='" + URLEncode([District]) + "'>" + [District] + " Dashboard</a>"
```

**Parameters:**
- `sType=wid`: Web Intelligence document
- `sDocName`: Target document name
- `lsS[PromptName]:=`: Single-value prompt
- `lsM[PromptName]:=`: Multi-value prompt

**Always use `URLEncode()` for parameter values!**

---

# PART 4: POLICE DATA PATTERNS

**This is a specialized section for handling a common challenge in police data: multiple entries per ID.**

## The Problem

Police databases often contain multiple records for the same entity (Case ID, Incident ID, Person ID, etc.):
- Multiple updates/versions of the same case
- Draft vs. final reports
- Amended reports
- Data quality varies across records
- Different timestamps (created, modified, submitted)

**Goal:** Filter to the single "best" record per ID based on various criteria.

---

## 4.1 Filtering Multiple Entries per ID

### Understanding the Challenge

**Scenario:** You have a Case table with multiple records per Case ID:

| Case ID | Report Date | Modified Date | Status | Priority | Notes | Evidence Count |
|---------|-------------|---------------|--------|----------|-------|----------------|
| C-2024-001 | 2024-01-15 | 2024-01-15 | Draft | 5 | Initial | NULL |
| C-2024-001 | 2024-01-16 | 2024-01-20 | Final | 7 | Complete | 3 |
| C-2024-001 | 2024-01-17 | 2024-01-25 | Amended | 7 | Updated | 5 |
| C-2024-002 | 2024-01-18 | 2024-01-18 | Final | 3 | Standard | 1 |

**Without filtering, aggregations give incorrect results:**
- `Count([Case ID])` returns 4, not 2 unique cases
- `Sum([Evidence Count])` may double-count

**Solution Approach:**
1. Identify what makes a record the "best" (latest date, most complete, highest priority, etc.)
2. Create a flag variable to mark the "best" record per ID
3. Filter the report or use Where clauses to show only flagged records

---

## 4.2 Getting Latest/Most Recent Record

### Pattern 1: Latest by Single Date Field

**Goal:** Get the most recent record per Case ID based on Report Date.

**Step 1: Find the maximum date per Case ID**

```webi
// Variable: var_Max_Report_Date_Per_Case
// Qualification: Detail or Measure
Max([Report Date]) In ([Case ID])
```

**Step 2: Flag the latest record**

```webi
// Variable: var_Is_Latest_Report
// Qualification: Dimension
If [Report Date] = Max([Report Date]) In ([Case ID]) Then "Yes" Else "No"
```

**Step 3: Use in report**

Apply report filter: `[var_Is_Latest_Report] = "Yes"`

Or use in aggregations:
```webi
// Count only latest records
Count([Case ID]) Where ([var_Is_Latest_Report] = "Yes")
```

---

### Pattern 2: Latest by Multiple Date Fields (Priority Order)

**Goal:** Use Modified Date if available, otherwise Report Date.

```webi
// Variable: var_Effective_Date
// Qualification: Detail
If Not IsNull([Modified Date]) Then [Modified Date] Else [Report Date]
```

```webi
// Variable: var_Max_Effective_Date_Per_Case
Max([var_Effective_Date]) In ([Case ID])
```

```webi
// Variable: var_Is_Latest_Version
// Qualification: Dimension
If [var_Effective_Date] = Max([var_Effective_Date]) In ([Case ID]) Then "Yes" Else "No"
```

---

### Pattern 3: Latest by Timestamp

**Goal:** When you have date + time fields.

```webi
// Variable: var_Full_Timestamp
// Qualification: Detail
// Combine date and time for precise comparison
[Report Date] + ([Report Time] / 86400)
// Note: Time stored as seconds, divided by 86400 (seconds per day)
```

```webi
// Variable: var_Latest_Timestamp_Per_Case
Max([var_Full_Timestamp]) In ([Case ID])
```

```webi
// Variable: var_Is_Latest_By_Timestamp
If [var_Full_Timestamp] = Max([var_Full_Timestamp]) In ([Case ID]) Then "Yes" Else "No"
```

---

### Complete Example: Latest Case Record

**Scenario:** Case table with multiple versions per case. Get the latest version only.

```webi
// Step 1: Create helper variable for latest date per case
// Variable: var_Latest_Case_Date
// Qualification: Detail
Max([Modified Date]) In ([Case ID])

// Step 2: Flag the latest record
// Variable: var_Is_Latest_Case_Record
// Qualification: Dimension
If [Modified Date] = [var_Latest_Case_Date] Then "Latest" Else "Historical"

// Step 3: Filter report
// Report Filter: [var_Is_Latest_Case_Record] = "Latest"
```

**Result:** Only one record per Case ID (the most recent).

---

## 4.3 Getting Most Complete Record

### Pattern 1: Count Non-Null Fields

**Goal:** Select the record with the fewest null/missing values.

```webi
// Variable: var_Completeness_Score
// Qualification: Measure
// Count how many key fields are populated
(If Not IsNull([Field1]) Then 1 Else 0) +
(If Not IsNull([Field2]) Then 1 Else 0) +
(If Not IsNull([Field3]) Then 1 Else 0) +
(If Not IsNull([Field4]) Then 1 Else 0) +
(If Not IsNull([Field5]) Then 1 Else 0)
```

**Simpler version for specific fields:**

```webi
// Variable: var_Data_Completeness_Score
// Count populated critical fields
(If Not IsNull([Suspect Name]) Then 1 Else 0) +
(If Not IsNull([Location]) Then 1 Else 0) +
(If Not IsNull([Evidence Count]) And [Evidence Count] > 0 Then 1 Else 0) +
(If Not IsNull([Witness Count]) And [Witness Count] > 0 Then 1 Else 0) +
(If Not IsNull([Officer Assigned]) Then 1 Else 0)
```

```webi
// Variable: var_Max_Completeness_Per_Case
Max([var_Data_Completeness_Score]) In ([Case ID])
```

```webi
// Variable: var_Is_Most_Complete
// Qualification: Dimension
If [var_Data_Completeness_Score] = Max([var_Data_Completeness_Score]) In ([Case ID])
Then "Most Complete"
Else "Incomplete"
```

---

### Pattern 2: Weighted Completeness Score

**Goal:** Some fields are more important than others.

```webi
// Variable: var_Weighted_Completeness
// Higher weight for critical fields
(If Not IsNull([Suspect Name]) Then 5 Else 0) +           // Critical
(If Not IsNull([Location]) Then 5 Else 0) +               // Critical
(If Not IsNull([Crime Type]) Then 3 Else 0) +             // Important
(If Not IsNull([Evidence Count]) Then 3 Else 0) +         // Important
(If Not IsNull([Notes]) And Length([Notes]) > 20 Then 2 Else 0) +  // Nice to have
(If Not IsNull([Reporting Officer]) Then 1 Else 0)        // Basic
```

```webi
// Variable: var_Max_Weighted_Score_Per_Case
Max([var_Weighted_Completeness]) In ([Case ID])
```

```webi
// Variable: var_Is_Most_Complete_Weighted
If [var_Weighted_Completeness] = Max([var_Weighted_Completeness]) In ([Case ID])
Then "Best Record"
Else "Lower Quality"
```

---

### Pattern 3: String Length as Quality Indicator

**Goal:** Longer descriptions usually indicate more detail.

```webi
// Variable: var_Description_Quality
// Longer case notes = better quality
Length([Case Notes])
```

```webi
// Variable: var_Best_Description_Per_Case
Max(Length([Case Notes])) In ([Case ID])
```

```webi
// Variable: var_Has_Best_Description
If Length([Case Notes]) = Max(Length([Case Notes])) In ([Case ID])
Then "Best"
Else "Other"
```

---

## 4.4 Getting Last Modified Record

### Pattern 1: Simple Last Modified

```webi
// Variable: var_Latest_Modified_Date_Per_Case
Max([Modified Date]) In ([Case ID])
```

```webi
// Variable: var_Is_Last_Modified
If [Modified Date] = Max([Modified Date]) In ([Case ID]) Then "Yes" Else "No"
```

---

### Pattern 2: Last Modified with User Tracking

**Goal:** Track WHO made the last modification.

```webi
// Variable: var_Latest_Modified_Date
Max([Modified Date]) In ([Case ID])
```

```webi
// Variable: var_Is_Last_Modifier
// Get the user who made the last modification
If [Modified Date] = Max([Modified Date]) In ([Case ID]) Then [Modified By User] Else ""
```

---

### Pattern 3: Modified Date with Status Check

**Goal:** Get latest FINALIZED record (ignore drafts).

```webi
// Variable: var_Latest_Final_Date
Max([Modified Date]) Where ([Status] = "Final") In ([Case ID])
```

```webi
// Variable: var_Is_Latest_Final_Record
If [Status] = "Final" And [Modified Date] = Max([Modified Date]) Where ([Status] = "Final") In ([Case ID])
Then "Latest Final"
Else "Other"
```

---

## 4.5 Priority-Based Record Selection

### Pattern 1: Status Priority

**Goal:** Prefer certain statuses over others (Final > Amended > Draft).

```webi
// Variable: var_Status_Priority
// Assign numeric priority to status values
If [Status] = "Amended" Then 3
ElseIf [Status] = "Final" Then 2
ElseIf [Status] = "Draft" Then 1
Else 0
```

```webi
// Variable: var_Highest_Priority_Per_Case
Max([var_Status_Priority]) In ([Case ID])
```

```webi
// Variable: var_Is_Highest_Priority_Status
If [var_Status_Priority] = Max([var_Status_Priority]) In ([Case ID])
Then "Best Status"
Else "Lower Priority Status"
```

---

### Pattern 2: Multi-Criteria Priority

**Goal:** Use multiple criteria in priority order (Status first, then date, then completeness).

```webi
// Variable: var_Composite_Priority_Score
// Higher score = better record
// Format: SSDDDDCC (Status=2 digits, Date=4 digits offset, Completeness=2 digits)
([var_Status_Priority] * 1000000) +
(DaysBetween(ToDate("2020-01-01"; "yyyy-MM-dd"); [Modified Date]) * 100) +
[var_Completeness_Score]
```

```webi
// Variable: var_Max_Priority_Score_Per_Case
Max([var_Composite_Priority_Score]) In ([Case ID])
```

```webi
// Variable: var_Is_Best_Record
If [var_Composite_Priority_Score] = Max([var_Composite_Priority_Score]) In ([Case ID])
Then "Best"
Else "Not Best"
```

**How it works:**
- Status Priority (3) = 3,000,000 points
- Days since 2020 (1,825 days) = 182,500 points
- Completeness (5 fields) = 5 points
- Total: 3,182,505

This ensures Status is prioritized first, then recent date, then completeness.

---

### Pattern 3: Source System Priority

**Goal:** Prefer records from certain source systems.

```webi
// Variable: var_Source_Priority
If [Source System] = "RMS Primary" Then 10
ElseIf [Source System] = "RMS Secondary" Then 5
ElseIf [Source System] = "Manual Entry" Then 3
ElseIf [Source System] = "Legacy Import" Then 1
Else 0
```

```webi
// Variable: var_Best_Source_Per_Case
Max([var_Source_Priority]) In ([Case ID])
```

```webi
// Variable: var_Is_From_Best_Source
If [var_Source_Priority] = Max([var_Source_Priority]) In ([Case ID])
Then "Primary Source"
Else "Secondary Source"
```

---

## 4.6 Data Quality Scoring

### Pattern 1: Comprehensive Quality Score

**Goal:** Score each record on multiple quality dimensions.

```webi
// Variable: var_Data_Quality_Score
// Qualification: Measure

// Completeness (0-30 points)
(If Not IsNull([Suspect Name]) Then 5 Else 0) +
(If Not IsNull([Location]) Then 5 Else 0) +
(If Not IsNull([Crime Type]) Then 5 Else 0) +
(If Not IsNull([Date]) Then 5 Else 0) +
(If Not IsNull([Officer]) Then 5 Else 0) +
(If Not IsNull([Evidence Count]) And [Evidence Count] > 0 Then 5 Else 0) +

// Timeliness (0-20 points)
(If DaysBetween([Incident Date]; [Report Date]) <= 1 Then 20
 ElseIf DaysBetween([Incident Date]; [Report Date]) <= 7 Then 10
 ElseIf DaysBetween([Incident Date]; [Report Date]) <= 30 Then 5
 Else 0) +

// Detail Level (0-20 points)
(If Length([Case Notes]) > 200 Then 20
 ElseIf Length([Case Notes]) > 100 Then 10
 ElseIf Length([Case Notes]) > 50 Then 5
 Else 0) +

// Status (0-30 points)
(If [Status] = "Amended" Then 30
 ElseIf [Status] = "Final" Then 25
 ElseIf [Status] = "Submitted" Then 15
 ElseIf [Status] = "Draft" Then 5
 Else 0)

// Maximum possible score: 100 points
```

```webi
// Variable: var_Highest_Quality_Per_Case
Max([var_Data_Quality_Score]) In ([Case ID])
```

```webi
// Variable: var_Is_Highest_Quality
If [var_Data_Quality_Score] = Max([var_Data_Quality_Score]) In ([Case ID])
Then "Highest Quality"
Else "Lower Quality"
```

```webi
// Variable: var_Quality_Rating
// Convert score to rating
If [var_Data_Quality_Score] >= 80 Then "Excellent"
ElseIf [var_Data_Quality_Score] >= 60 Then "Good"
ElseIf [var_Data_Quality_Score] >= 40 Then "Fair"
ElseIf [var_Data_Quality_Score] >= 20 Then "Poor"
Else "Very Poor"
```

---

### Pattern 2: Validation Flags

**Goal:** Flag records with data quality issues.

```webi
// Variable: var_Has_Data_Issues
// Qualification: Dimension

If IsNull([Case Type]) Then "Missing Case Type"
ElseIf IsNull([Location]) Then "Missing Location"
ElseIf IsNull([Officer Assigned]) Then "Unassigned"
ElseIf [Evidence Count] = 0 And DaysBetween([Incident Date]; CurrentDate()) > 30 Then "No Evidence After 30 Days"
ElseIf DaysBetween([Incident Date]; [Report Date]) > 7 Then "Late Report"
Else "No Issues"
```

```webi
// Variable: var_Quality_Flag_Count
// Count quality issues
(If IsNull([Case Type]) Then 1 Else 0) +
(If IsNull([Location]) Then 1 Else 0) +
(If IsNull([Officer Assigned]) Then 1 Else 0) +
(If [Evidence Count] = 0 Then 1 Else 0) +
(If DaysBetween([Incident Date]; [Report Date]) > 7 Then 1 Else 0)
```

```webi
// Variable: var_Fewest_Issues_Per_Case
Min([var_Quality_Flag_Count]) In ([Case ID])
```

```webi
// Variable: var_Is_Cleanest_Record
If [var_Quality_Flag_Count] = Min([var_Quality_Flag_Count]) In ([Case ID])
Then "Cleanest"
Else "Has Issues"
```

---

## 4.7 Combined Filtering Strategy

### Complete Working Example: Best Record Selection

**Scenario:** Case table with multiple versions. Select the single best record per case using comprehensive criteria.

```webi
// ========================================
// STEP 1: Calculate individual criteria
// ========================================

// 1A: Status Priority
// Variable: var_Status_Priority
// Qualification: Measure
If [Status] = "Amended" Then 3
ElseIf [Status] = "Final" Then 2
ElseIf [Status] = "Draft" Then 1
Else 0

// 1B: Data Completeness
// Variable: var_Completeness_Count
// Qualification: Measure
(If Not IsNull([Suspect Name]) Then 1 Else 0) +
(If Not IsNull([Location]) Then 1 Else 0) +
(If Not IsNull([Evidence Count]) And [Evidence Count] > 0 Then 1 Else 0) +
(If Not IsNull([Witness Count]) And [Witness Count] > 0 Then 1 Else 0) +
(If Not IsNull([Case Notes]) And Length([Case Notes]) > 20 Then 1 Else 0)

// 1C: Recency Score (days since base date)
// Variable: var_Recency_Score
// Qualification: Measure
DaysBetween(ToDate("2020-01-01"; "yyyy-MM-dd"); [Modified Date])

// ========================================
// STEP 2: Create composite ranking score
// ========================================

// Variable: var_Record_Rank_Score
// Qualification: Measure
// Priority order: Status > Date > Completeness
([var_Status_Priority] * 10000000) +
([var_Recency_Score] * 100) +
[var_Completeness_Count]

// ========================================
// STEP 3: Find best score per Case ID
// ========================================

// Variable: var_Best_Score_Per_Case
// Qualification: Detail or Measure
Max([var_Record_Rank_Score]) In ([Case ID])

// ========================================
// STEP 4: Flag the best record
// ========================================

// Variable: var_Is_Best_Record
// Qualification: Dimension
If [var_Record_Rank_Score] = [var_Best_Score_Per_Case] Then "BEST" Else "Other"

// ========================================
// STEP 5: Apply filter
// ========================================
// Report Filter: [var_Is_Best_Record] = "BEST"
```

---

### Handling Ties

**Problem:** Multiple records have the same "best" score.

**Solution 1: Add tie-breaker**

```webi
// Variable: var_Record_Rank_With_Tiebreaker
([var_Status_Priority] * 10000000) +
([var_Recency_Score] * 100) +
([var_Completeness_Count] * 10) +
([Record ID])  // Use unique ID as final tie-breaker
```

**Solution 2: Keep all tied records**

```webi
// Variable: var_Is_Best_Or_Tied
If [var_Record_Rank_Score] = Max([var_Record_Rank_Score]) In ([Case ID])
Then "Keep"
Else "Exclude"
```

**Solution 3: Prefer specific attribute in tie**

```webi
// Variable: var_Tiebreaker
// If scores are tied, prefer records modified by supervisors
If [var_Record_Rank_Score] = Max([var_Record_Rank_Score]) In ([Case ID]) Then
    If [Modified By Role] = "Supervisor" Then 1 Else 0
Else 0
```

---

## 4.8 Performance Optimization Tips

### Tip 1: Use Query Filters When Possible

If you can identify "best" records at the database level:

```sql
-- Example SQL in universe
SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY case_id ORDER BY modified_date DESC) as rn
    FROM case_table
)
WHERE rn = 1
```

Then filter in WebI query:
```
Object: [Row Number]
Operator: Equal to
Value: 1
```

---

### Tip 2: Pre-calculate Scores in Database

Create calculated fields in the universe for complex scoring logic.

**Universe Object:** `Best_Record_Score`
```sql
(CASE status
    WHEN 'Amended' THEN 3000000
    WHEN 'Final' THEN 2000000
    WHEN 'Draft' THEN 1000000
    ELSE 0
END) +
(DATEDIFF(day, '2020-01-01', modified_date) * 100) +
(completeness_score)
```

Then in WebI:
```webi
Max([Best Record Score]) In ([Case ID])
```

---

### Tip 3: Use Report Filters vs Variables for Large Datasets

For very large datasets (>100K records), apply filters at report level rather than using Where clauses in every variable:

**Better:**
```
Report Filter: [var_Is_Best_Record] = "BEST"
Then all measures automatically filtered
```

**Slower:**
```webi
Count([Case ID]) Where ([var_Is_Best_Record] = "BEST")
Sum([Evidence Count]) Where ([var_Is_Best_Record] = "BEST")
// Repeated for every measure
```

---

## 4.9 Common Patterns Summary

### Quick Reference Table

| Goal | Key Formula Pattern | Use When |
|------|---------------------|----------|
| **Latest Record** | `If [Date] = Max([Date]) In ([ID]) Then "Yes"` | Simple date-based versioning |
| **Most Complete** | `Max([Completeness Score]) In ([ID])` | Variable data quality |
| **Last Modified** | `Max([Modified Date]) In ([ID])` | Audit trail exists |
| **Status Priority** | `Max([Status Numeric Priority]) In ([ID])` | Status hierarchy exists |
| **Multi-Criteria** | `Max([Composite Score]) In ([ID])` | Multiple factors matter |
| **Data Quality** | `Max([Quality Score]) In ([ID])` | Comprehensive selection |

---

### Decision Tree: Which Pattern to Use?

```
Do you have modified/updated dates?
├─ YES → Do you care about data completeness?
│   ├─ YES → Use Multi-Criteria with Date + Completeness
│   └─ NO → Use Latest by Date
│
└─ NO → Do you have status values (Draft/Final)?
    ├─ YES → Do you care about completeness?
    │   ├─ YES → Use Status Priority + Completeness
    │   └─ NO → Use Status Priority Only
    │
    └─ NO → Do you have quality indicators?
        ├─ YES → Use Data Quality Score
        └─ NO → Use Most Complete Record
```

---

## 4.10 Troubleshooting

### Issue 1: Still Getting Multiple Records per ID

**Cause:** Tie in scoring - multiple records have same "best" score.

**Solution:** Add additional tie-breaker criteria or use unique record ID.

```webi
// Add record ID as tie-breaker
([Primary Score] * 1000) + [Record ID]
```

---

### Issue 2: #MULTIVALUE Error

**Cause:** Selected field varies across "best" records for an ID.

**Solution:** Either:
1. Add the varying dimension to your block
2. Use aggregation: `Max([Field]) In ([ID])`
3. Ensure your "best record" logic accounts for this field

---

### Issue 3: Wrong Record Selected

**Cause:** Scoring logic doesn't match business rules.

**Debug:** Add these variables to your report temporarily:

```webi
// Variable: var_Debug_Score
[var_Status_Priority] + " | " +
"" + [var_Recency_Score] + " | " +
"" + [var_Completeness_Count]
```

Review scores side-by-side with actual records to validate logic.

---

### Issue 4: Performance is Slow

**Solutions:**
1. Move filtering to query level if possible
2. Pre-calculate scores in universe
3. Use report filters instead of Where clauses
4. Reduce number of records with query filters first
5. Index key date/ID fields in database

---

## 4.11 Real-World Example: Incident Reports

**Scenario:** Incident table has multiple reports per incident (initial, follow-up, amended).

**Business Rule:** Use the latest FINAL or AMENDED report. If no final reports exist, use latest SUBMITTED. Never use DRAFT.

```webi
// Step 1: Assign status ranking
// Variable: var_Report_Status_Rank
If [Report Status] = "Amended" Then 4
ElseIf [Report Status] = "Final" Then 3
ElseIf [Report Status] = "Submitted" Then 2
ElseIf [Report Status] = "Draft" Then 1
Else 0

// Step 2: Combine status rank with date
// Variable: var_Report_Selection_Score
// Format: RDDDD (Rank + Days since 2020)
([var_Report_Status_Rank] * 10000) +
DaysBetween(ToDate("2020-01-01"; "yyyy-MM-dd"); [Report Date])

// Step 3: Find best score per incident
// Variable: var_Best_Report_Score_Per_Incident
Max([var_Report_Selection_Score]) In ([Incident ID])

// Step 4: Flag best report
// Variable: var_Is_Selected_Report
If [var_Report_Selection_Score] = [var_Best_Report_Score_Per_Incident]
Then "Selected"
Else "Not Selected"

// Step 5: Exclude drafts entirely
// Variable: var_Final_Selection
If [Report Status] = "Draft" Then "Exclude"
ElseIf [var_Is_Selected_Report] = "Selected" Then "Include"
Else "Exclude"

// Apply Report Filter: [var_Final_Selection] = "Include"
```

**Result:** One report per Incident ID, preferring Amended > Final > Submitted, latest date wins ties, drafts excluded.

---



