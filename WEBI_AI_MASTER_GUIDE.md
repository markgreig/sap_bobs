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

# Part 5: Problem-Solution Mapping

This section provides quick solutions to common WebI problems, organized by task type. Each problem includes context, solution, and police data example.

## 5.1 Data Quality & Filtering

### Problem 5.1.1: Get Only the Latest Record per ID

**Context:** Multiple entries per Case ID, need only the most recent.

**Solution Pattern:**
```webi
// Step 1: Find latest date per ID
// Variable: var_Latest_Date_Per_Case
Max([Incident Date]) In ([Case ID])

// Step 2: Flag latest records
// Variable: var_Is_Latest
If [Incident Date] = [var_Latest_Date_Per_Case] Then "Latest" Else "Historical"

// Step 3: Filter
// Report Filter: [var_Is_Latest] = "Latest"
```

**See Also:** Part 4.2 - Getting Latest/Most Recent Record

---

### Problem 5.1.2: Get Only Records Modified in Last 30 Days

**Context:** Focus analysis on recently updated cases only.

**Solution Pattern:**
```webi
// Query Filter (preferred - faster):
[Modified Date] >= RelativeDate(CurrentDate(); -30)

// OR Variable approach:
// Variable: var_Recent_Activity
If [Modified Date] >= RelativeDate(CurrentDate(); -30)
Then "Recent"
Else "Old"

// Report Filter: [var_Recent_Activity] = "Recent"
```

**Performance Note:** Query filter is 10-100x faster than report filter for large datasets.

---

### Problem 5.1.3: Exclude Records with Missing Critical Fields

**Context:** Need complete records only - exclude cases missing officer assignment or location.

**Solution Pattern:**
```webi
// Variable: var_Is_Complete_Record
If Not(IsNull([Assigned Officer])) And Not(IsNull([Location]))
Then "Complete"
Else "Incomplete"

// Report Filter: [var_Is_Complete_Record] = "Complete"

// OR single-line version:
If IsNull([Assigned Officer]) Or IsNull([Location]) Then "Exclude" Else "Include"
```

**See Also:** Part 4.3 - Getting Most Complete Record

---

### Problem 5.1.4: Get Best Record Based on Status Priority

**Context:** Multiple records per Case ID with different statuses - prefer "Closed" over "Active" over "Pending".

**Solution Pattern:**
```webi
// Step 1: Assign numeric priority
// Variable: var_Status_Priority
If [Case Status] = "Closed" Then 3
ElseIf [Case Status] = "Active" Then 2
ElseIf [Case Status] = "Pending" Then 1
Else 0

// Step 2: Find highest priority per case
// Variable: var_Best_Priority_Per_Case
Max([var_Status_Priority]) In ([Case ID])

// Step 3: Flag best record
// Variable: var_Is_Best_Status
If [var_Status_Priority] = [var_Best_Priority_Per_Case]
Then "Best"
Else "Other"

// Report Filter: [var_Is_Best_Status] = "Best"
```

**See Also:** Part 4.5 - Priority-Based Record Selection

---

### Problem 5.1.5: Filter to Show Only Records Above Threshold

**Context:** Show only high-priority cases (priority >= 7) or high-value incidents (value > $10,000).

**Solution Pattern:**
```webi
// Query Filter (preferred):
[Priority Level] >= 7

// OR Variable approach:
// Variable: var_Priority_Category
If [Priority Level] >= 7 Then "High Priority" Else "Normal"

// Report Filter: [var_Priority_Category] = "High Priority"

// Multiple conditions:
If [Priority Level] >= 7 And [Property Value] > 10000
Then "Critical High Value"
Else "Other"
```

---

## 5.2 Calculations & Aggregations

### Problem 5.2.1: Calculate Percentage of Total

**Context:** Show each district's arrest count as percentage of total arrests.

**Solution Pattern:**
```webi
// Variable: var_Arrest_Percentage
// Qualification: Measure
([District Arrests] / Sum([District Arrests]) ForAll([District])) * 100

// With formatting:
FormatNumber(
  ([District Arrests] / Sum([District Arrests]) ForAll([District])) * 100,
  "0.00"
) + "%"
```

**Key Concept:** `ForAll([District])` removes district from context, giving total.

**See Also:** Part 3.3.3 - ForAll operator

---

### Problem 5.2.2: Calculate Running Total

**Context:** Show cumulative arrests by month throughout the year.

**Solution Pattern:**
```webi
// Variable: var_Cumulative_Arrests
// Qualification: Measure
RunningSum([Arrests])

// IMPORTANT: Block must be sorted by date ascending
```

**Requirements:**
- Block must have proper sort order
- Works best with single dimension (Month/Date)

**See Also:** Part 3.4.8 - RunningSum function

---

### Problem 5.2.3: Calculate Month-over-Month Change

**Context:** Compare current month arrests to previous month.

**Solution Pattern:**
```webi
// Variable: var_MoM_Arrest_Change
// Qualification: Measure
[Arrests] - Previous([Arrests])

// Percentage change:
// Variable: var_MoM_Arrest_Change_Pct
If IsNull(Previous([Arrests])) Or Previous([Arrests]) = 0
Then 0
Else (([Arrests] - Previous([Arrests])) / Previous([Arrests])) * 100
```

**Requirements:**
- Block must be sorted by time dimension
- Use `IsNull()` check to handle first period

**See Also:** Part 3.7.9 - Previous function

---

### Problem 5.2.4: Count Distinct Cases (Not Total Records)

**Context:** Dataset has multiple records per case - need unique case count.

**Solution Pattern:**
```webi
// Variable: var_Unique_Case_Count
// Qualification: Measure
Count([Case ID]; Distinct)

// VS regular count (counts all records):
Count([Case ID])
```

**When to Use:** Whenever data has multiple rows per entity and you need entity count.

**See Also:** Part 3.2.3 - Count with Distinct

---

### Problem 5.2.5: Calculate Average Excluding Outliers

**Context:** Calculate average response time but exclude extreme outliers (>2 hours).

**Solution Pattern:**
```webi
// Variable: var_Response_Time_Minutes_Filtered
// Qualification: Measure
Average([Response Time Minutes]) Where ([Response Time Minutes] <= 120)
```

**Alternative - Using Variables:**
```webi
// Step 1: Flag outliers
// Variable: var_Is_Normal_Response
If [Response Time Minutes] <= 120 Then "Normal" Else "Outlier"

// Step 2: Calculate average on filtered data
// Apply Report Filter: [var_Is_Normal_Response] = "Normal"
// Then use: Average([Response Time Minutes])
```

---

### Problem 5.2.6: Create Conditional Sum (Sum with Filter)

**Context:** Sum only arrests where arrest type is "Felony".

**Solution Pattern:**
```webi
// Variable: var_Felony_Arrest_Count
// Qualification: Measure
Sum([Arrests]) Where ([Arrest Type] = "Felony")

// Multiple conditions:
Sum([Arrests]) Where ([Arrest Type] = "Felony" And [District] = "North")
```

**Limitation:** WHERE clause right side must be constant - cannot use `Where ([District] = [Selected District])`.

**See Also:** Part 3.3.4 - Where operator

---

### Problem 5.2.7: Calculate Weighted Average

**Context:** Calculate district average response time weighted by case volume.

**Solution Pattern:**
```webi
// Variable: var_Weighted_Avg_Response_Time
// Qualification: Measure
Sum([Response Time Minutes] * [Case Count]) / Sum([Case Count])
```

**Example Values:**
- District A: 15 min response, 100 cases → weight = 1500
- District B: 25 min response, 50 cases → weight = 1250
- Weighted average: (1500 + 1250) / (100 + 50) = 18.33 minutes

---

## 5.3 Date & Time Analysis

### Problem 5.3.1: Get Records from Last N Days

**Context:** Show cases opened in last 7 days.

**Solution Pattern:**
```webi
// Query Filter (preferred):
[Open Date] >= RelativeDate(CurrentDate(); -7)

// Variable approach:
// Variable: var_Is_Recent_Case
If [Open Date] >= RelativeDate(CurrentDate(); -7)
Then "Last 7 Days"
Else "Older"
```

**Common Timeframes:**
- Last 7 days: `RelativeDate(CurrentDate(); -7)`
- Last 30 days: `RelativeDate(CurrentDate(); -30)`
- Last 90 days: `RelativeDate(CurrentDate(); -90)`
- Last year: `RelativeDate(CurrentDate(); -365)`

---

### Problem 5.3.2: Calculate Days Between Two Dates

**Context:** Calculate how many days a case has been open.

**Solution Pattern:**
```webi
// Variable: var_Days_Open
// Qualification: Measure
DaysBetween([Open Date]; CurrentDate())

// If case has close date, use that:
// Variable: var_Days_To_Close
If IsNull([Close Date])
Then DaysBetween([Open Date]; CurrentDate())
Else DaysBetween([Open Date]; [Close Date])
```

**See Also:** Part 3.4.6 - DaysBetween function

---

### Problem 5.3.3: Group Dates into Weeks/Months/Quarters

**Context:** Aggregate daily incident data into weekly or monthly views.

**Solution Pattern:**
```webi
// Week (showing start of week):
// Variable: var_Week_Start
// Qualification: Dimension
RelativeDate([Incident Date]; -(DayNumberOfWeek([Incident Date]) - 1))

// Month name:
// Variable: var_Month_Name
// Qualification: Dimension
MonthName([Incident Date])

// Year-Month combined:
// Variable: var_Year_Month
// Qualification: Dimension
FormatDate([Incident Date]; "yyyy-MM")

// Quarter:
// Variable: var_Quarter
// Qualification: Dimension
"Q" + Quarter([Incident Date]) + " " + Year([Incident Date])
```

---

### Problem 5.3.4: Identify Business Days vs Weekends

**Context:** Separate incidents occurring on weekdays from weekend incidents.

**Solution Pattern:**
```webi
// Variable: var_Day_Type
// Qualification: Dimension
If DayNumberOfWeek([Incident Date]) In (1; 7)
Then "Weekend"
Else "Weekday"

// More detailed:
// Variable: var_Day_Category
If DayNumberOfWeek([Incident Date]) = 1 Then "Sunday"
ElseIf DayNumberOfWeek([Incident Date]) = 7 Then "Saturday"
ElseIf DayNumberOfWeek([Incident Date]) = 6 Then "Friday"
Else "Mon-Thu"
```

**DayNumberOfWeek Values:** 1=Sunday, 2=Monday, ..., 7=Saturday

---

### Problem 5.3.5: Calculate Time Between Timestamps

**Context:** Calculate response time in hours/minutes between dispatch and arrival.

**Solution Pattern:**
```webi
// Hours (decimal):
// Variable: var_Response_Hours
// Qualification: Measure
([Arrival Time] - [Dispatch Time]) * 24

// Minutes:
// Variable: var_Response_Minutes
// Qualification: Measure
([Arrival Time] - [Dispatch Time]) * 24 * 60

// Formatted as HH:MM:
// Variable: var_Response_Time_Formatted
FormatNumber(Floor(([Arrival Time] - [Dispatch Time]) * 24); "00") + ":" +
FormatNumber(Round((([Arrival Time] - [Dispatch Time]) * 24 * 60) Mod 60; 0); "00")
```

---

### Problem 5.3.6: Get Year-to-Date (YTD) Total

**Context:** Show total arrests from January 1st of current year to today.

**Solution Pattern:**
```webi
// Variable: var_YTD_Arrests
// Qualification: Measure
Sum([Arrests]) Where (
  [Arrest Date] >= ToDate("01/01/" + Year(CurrentDate()); "MM/dd/yyyy")
  And [Arrest Date] <= CurrentDate()
)
```

**Alternative Using RelativeDate:**
```webi
// Approximate YTD (last 365 days):
Sum([Arrests]) Where ([Arrest Date] >= RelativeDate(CurrentDate(); -365))
```

---

## 5.4 Report Structure & Display

### Problem 5.4.1: Create Categorical Labels from Numeric Values

**Context:** Convert numeric priority (1-10) into categories (Low/Medium/High).

**Solution Pattern:**
```webi
// Variable: var_Priority_Category
// Qualification: Dimension
If [Priority Score] >= 8 Then "High"
ElseIf [Priority Score] >= 4 Then "Medium"
Else "Low"
```

**Best Practice:** Use Dimension qualification for categorical text results.

**See Also:** Part 2.1 - Variable Qualification Types

---

### Problem 5.4.2: Combine Multiple Fields into Single Display

**Context:** Show officer name as "LastName, FirstName (Badge#)".

**Solution Pattern:**
```webi
// Variable: var_Officer_Full_Display
// Qualification: Detail
[Officer Last Name] + ", " + [Officer First Name] + " (" + [Badge Number] + ")"
```

**Result Example:** "Johnson, Michael (1247)"

**See Also:** Part 3.5.1 - String concatenation

---

### Problem 5.4.3: Format Numbers with Thousands Separators

**Context:** Display case count as "1,234" instead of "1234".

**Solution Pattern:**
```webi
// Variable: var_Case_Count_Formatted
// Qualification: Dimension (for display text)
FormatNumber([Case Count]; "#,##0")

// With decimals:
FormatNumber([Average Response Time]; "#,##0.00")

// Currency:
FormatNumber([Property Value]; "$#,##0.00")
```

**See Also:** Part 3.6.1 - FormatNumber function

---

### Problem 5.4.4: Show Rank Within Group

**Context:** Rank officers by arrest count within each district.

**Solution Pattern:**
```webi
// Variable: var_Officer_Rank_In_District
// Qualification: Measure
Rank([Arrests]; ([District]))

// Descending rank (highest first):
Rank([Arrests]; ([District]); "Descending")
```

**Requirements:**
- [District] must be in the block
- Block should be sorted by [Arrests] descending for visual clarity

**See Also:** Part 3.7.10 - Rank function

---

### Problem 5.4.5: Create Dynamic Hyperlinks to Case Details

**Context:** Link from report to external case management system.

**Solution Pattern:**
```webi
// Variable: var_Case_Detail_Link
// Qualification: Detail
"<a href='http://casemanagement.pd.local/case/" + [Case ID] + "'>" + [Case ID] + "</a>"
```

**OR using OpenDocument:**
```webi
// Variable: var_Case_Link_OpenDoc
"http://servername/opendocument.jsp?iDocID=12345&lsSCase_ID=" + [Case ID]
```

**Note:** OpenDocument syntax varies by BI platform version.

---

## 5.5 Performance & Optimization

### Problem 5.5.1: Report is Slow - How Do I Speed It Up?

**Context:** Report takes too long to run or refresh.

**Solution Checklist:**

1. **Move filters to query level (10-100x faster)**
   ```webi
   // SLOW (Report Filter):
   [District] = "North"

   // FAST (Query Filter):
   Move to Edit Query → Query Filters
   ```

2. **Remove unused objects from query**
   - Edit Query → Remove any fields not used in report

3. **Limit Scope of Analysis**
   - Query Properties → Scope of Analysis → "None" (if no drilling needed)

4. **Use aggregated data providers**
   - Pre-aggregate at database level if possible

5. **Minimize merged dimensions**
   - Merging is expensive - only merge when necessary

6. **Add database indexes**
   - Ensure filter fields and join keys are indexed

**See Also:** Part 7 - Optimization Guide, Part 4.8 - Performance Optimization

---

### Problem 5.5.2: Formula is Causing #COMPUTATION Error

**Context:** Formula works in some blocks but fails with #COMPUTATION in others.

**Solution Pattern:**
```webi
// PROBLEM: Missing context dimension
// This fails if [District] not in block:
Max([Arrests])

// SOLUTION: Explicitly specify context
Max([Arrests]) In ([Officer ID]; [Month])

// OR use ForEach to add dimension:
Max([Arrests]) ForEach([District])
```

**Root Cause:** WebI can't determine aggregation level without proper context.

**See Also:** Part 6.2 - #COMPUTATION error, Part 2.2 - Calculation Contexts

---

### Problem 5.5.3: Need to Ignore Report Filters for Specific Calculation

**Context:** Want to show "% of Total" where total is unfiltered, but detail rows are filtered.

**Solution Pattern:**
```webi
// Variable: var_Filtered_Arrests
// Qualification: Measure
Sum([Arrests])  // This respects filters

// Variable: var_Total_Arrests_Unfiltered
// Qualification: Measure
NoFilter(Sum([Arrests]))  // This ignores ALL report filters

// Variable: var_Percent_of_Unfiltered_Total
// Qualification: Measure
([var_Filtered_Arrests] / [var_Total_Arrests_Unfiltered]) * 100
```

**Use Case:** Showing filtered subset as percentage of complete dataset.

**See Also:** Part 3.7.11 - NoFilter function

---

## 5.6 Error Handling & Data Quality

### Problem 5.6.1: Prevent #DIV/0 Errors

**Context:** Division formula fails when denominator is zero.

**Solution Pattern:**
```webi
// Variable: var_Clearance_Rate
// Qualification: Measure
If [Cases Closed] = 0
Then 0
Else ([Cases Closed] / [Total Cases]) * 100

// More robust (handles null too):
If IsNull([Total Cases]) Or [Total Cases] = 0
Then 0
Else ([Cases Closed] / [Total Cases]) * 100
```

**Best Practice:** Always check denominator before division.

---

### Problem 5.6.2: Handle Null Values in Calculations

**Context:** Calculation fails or shows wrong results when fields contain nulls.

**Solution Pattern:**
```webi
// Replace null with zero:
// Variable: var_Arrests_Safe
If IsNull([Arrests]) Then 0 Else [Arrests]

// Replace null with default text:
// Variable: var_Officer_Name_Safe
If IsNull([Officer Name]) Then "Unassigned" Else [Officer Name]

// Skip nulls in conditional:
If Not(IsNull([Close Date])) And [Close Date] < CurrentDate()
Then "Closed On Time"
Else "Open or Late"
```

**See Also:** Part 3.7.8 - IsNull function

---

### Problem 5.6.3: Identify Records with Missing Data

**Context:** Find cases missing critical information for data quality audit.

**Solution Pattern:**
```webi
// Variable: var_Missing_Fields_Count
// Qualification: Measure
If IsNull([Officer Name]) Then 1 Else 0 +
If IsNull([Location]) Then 1 Else 0 +
If IsNull([Incident Type]) Then 1 Else 0

// Variable: var_Data_Quality_Status
// Qualification: Dimension
If [var_Missing_Fields_Count] = 0 Then "Complete"
ElseIf [var_Missing_Fields_Count] <= 2 Then "Mostly Complete"
Else "Incomplete"
```

**See Also:** Part 4.3 - Getting Most Complete Record

---

### Problem 5.6.4: Handle Errors Gracefully in Formulas

**Context:** Formula may error in some conditions - want to show 0 or default instead of error.

**Solution Pattern:**
```webi
// Variable: var_Safe_Calculation
// Qualification: Measure
If IsError([Complex Formula])
Then 0
Else [Complex Formula]

// With custom error message:
// Qualification: Dimension
If IsError([Complex Formula])
Then "Calculation Error"
Else FormatNumber([Complex Formula]; "#,##0.00")
```

**See Also:** Part 3.7.12 - IsError function

---

### Problem 5.6.5: Validate Data Ranges

**Context:** Flag records with suspicious values (negative durations, future dates, etc.).

**Solution Pattern:**
```webi
// Variable: var_Data_Validation_Flag
// Qualification: Dimension
If [Response Time Minutes] < 0 Then "ERROR: Negative Duration"
ElseIf [Response Time Minutes] > 1440 Then "WARNING: >24 hours"
ElseIf [Incident Date] > CurrentDate() Then "ERROR: Future Date"
ElseIf IsNull([Officer Name]) Then "WARNING: Missing Officer"
Else "Valid"

// Report Filter: [var_Data_Validation_Flag] Not In ("Valid")
// This shows only problematic records
```

---

## 5.7 Advanced Scenarios

### Problem 5.7.1: Create Dynamic Date Ranges Based on User Selection

**Context:** Let users choose "Last 7 Days", "Last 30 Days", "Last Quarter" dynamically.

**Solution Pattern:**
```webi
// Step 1: Create prompt variable
// Variable: var_Date_Range_Selection
// Use prompt: @Prompt('Select Time Period:', 'A', {'Last 7 Days', 'Last 30 Days', 'Last Quarter'}, mono, free)

// Step 2: Calculate date threshold
// Variable: var_Date_Threshold
If [var_Date_Range_Selection] = "Last 7 Days"
Then RelativeDate(CurrentDate(); -7)
ElseIf [var_Date_Range_Selection] = "Last 30 Days"
Then RelativeDate(CurrentDate(); -30)
ElseIf [var_Date_Range_Selection] = "Last Quarter"
Then RelativeDate(CurrentDate(); -90)
Else RelativeDate(CurrentDate(); -365)

// Step 3: Filter data
// Report Filter: [Incident Date] >= [var_Date_Threshold]
```

**See Also:** Part 2.3.7 - @Prompt function

---

### Problem 5.7.2: Compare Current Period to Same Period Last Year

**Context:** Show arrests this month vs same month last year.

**Solution Pattern:**
```webi
// Assuming separate data providers for current year and last year
// OR using a single provider with year dimension

// Variable: var_This_Year_Arrests
Sum([Arrests]) Where (Year([Arrest Date]) = Year(CurrentDate()))

// Variable: var_Last_Year_Arrests
Sum([Arrests]) Where (Year([Arrest Date]) = Year(CurrentDate()) - 1)

// Variable: var_YoY_Change
[var_This_Year_Arrests] - [var_Last_Year_Arrests]

// Variable: var_YoY_Change_Pct
If [var_Last_Year_Arrests] = 0
Then 0
Else (([var_This_Year_Arrests] - [var_Last_Year_Arrests]) / [var_Last_Year_Arrests]) * 100
```

---

### Problem 5.7.3: Create Multi-Level Categorization

**Context:** Categorize cases by multiple criteria: Priority + Case Type.

**Solution Pattern:**
```webi
// Variable: var_Case_Category_Detailed
// Qualification: Dimension
If [Priority Level] >= 8 And [Case Type] = "Violent Crime"
Then "CRITICAL: Violent"
ElseIf [Priority Level] >= 8 And [Case Type] = "Property Crime"
Then "CRITICAL: Property"
ElseIf [Priority Level] >= 8
Then "CRITICAL: Other"
ElseIf [Priority Level] >= 4 And [Case Type] = "Violent Crime"
Then "HIGH: Violent"
ElseIf [Priority Level] >= 4 And [Case Type] = "Property Crime"
Then "HIGH: Property"
ElseIf [Priority Level] >= 4
Then "HIGH: Other"
Else "ROUTINE"
```

**Result:** 7 distinct categories combining priority and type.

---

### Problem 5.7.4: Calculate Percentile Rank

**Context:** Determine what percentile each officer falls into by arrest count.

**Solution Pattern:**
```webi
// Variable: var_Officer_Percentile
// Qualification: Measure
(Rank([Arrests]; ()) / Count([Officer ID]; All; Distinct)) * 100

// Interpretation:
// 90 = Top 10% (90th percentile)
// 50 = Median
// 10 = Bottom 10%
```

**See Also:** Part 3.7.10 - Rank function

---

### Problem 5.7.5: Merge Data from Multiple Queries

**Context:** Combine case data from Query 1 with officer data from Query 2.

**Solution Pattern:**
1. **Create Merged Dimension:**
   - Right-click dimension in Available Objects
   - Choose "Merge"
   - Select matching dimension from other query
   - Example: Merge [Officer ID] from Query 1 with [Officer ID] from Query 2

2. **Use in Report:**
   ```webi
   // Block can now show:
   [Officer ID] (merged)
   [Cases] (from Query 1)
   [Officer Name] (from Query 2)
   ```

**Performance Note:** Merging is expensive - minimize merged dimensions.

**See Also:** Part 2.3.5 - Merged Dimensions

---

### Problem 5.7.6: Create Conditional Formatting Based on Formula

**Context:** Highlight cells red if response time exceeds threshold.

**Solution Pattern:**
```webi
// Create helper variable:
// Variable: var_Response_Time_Status
// Qualification: Dimension
If [Response Time Minutes] > 30 Then "Exceeded"
ElseIf [Response Time Minutes] > 20 Then "Warning"
Else "Normal"

// Then in report:
// 1. Right-click cell → Format Cell → Add conditional formatting
// 2. Condition: [var_Response_Time_Status] = "Exceeded"
// 3. Format: Background color = Red
```

**Alternative:** Use cell formatting rules directly on [Response Time Minutes] > 30.

---

### Problem 5.7.7: Show Top N Records Only

**Context:** Display only top 10 officers by arrest count.

**Solution Pattern:**
```webi
// Variable: var_Officer_Rank
// Qualification: Measure
Rank([Arrests]; (); "Descending")

// Report Filter: [var_Officer_Rank] <= 10
```

**Result:** Only top 10 officers displayed, dynamically updated as data changes.

**See Also:** Part 3.7.10 - Rank function

---

### Problem 5.7.8: Calculate Moving Average

**Context:** Show 7-day moving average of daily incidents.

**Solution Pattern:**
```webi
// Variable: var_7Day_Moving_Avg
// Qualification: Measure
Average([Daily Incidents]; (Previous(Self; 6); Previous(Self; 5); Previous(Self; 4);
Previous(Self; 3); Previous(Self; 2); Previous(Self; 1); Self))
```

**WebI 4.2 Note:** WebI doesn't have native moving average - requires complex Previous() nesting or database-level calculation.

**Alternative:** Use RunningSum with rolling window logic.

---

## 5.8 Quick Reference: Problem → Section Lookup

**Data Quality & Filtering:**
- Latest record per ID → Part 4.2
- Most complete record → Part 4.3
- Priority-based selection → Part 4.5
- Null handling → Problem 5.6.2

**Calculations:**
- Percentage of total → Problem 5.2.1
- Running total → Problem 5.2.2
- Month-over-month change → Problem 5.2.3
- Conditional sum → Problem 5.2.6

**Date & Time:**
- Last N days filter → Problem 5.3.1
- Days between dates → Problem 5.3.2
- Group by week/month → Problem 5.3.3
- YTD calculation → Problem 5.3.6

**Performance:**
- Slow report → Problem 5.5.1
- Query vs report filters → Part 2.3.4
- Optimization techniques → Part 7

**Errors:**
- #DIV/0 → Problem 5.6.1
- #COMPUTATION → Problem 5.5.2
- #MULTIVALUE → Part 6.1

---

# Part 6: Error Troubleshooting Guide

This section provides comprehensive troubleshooting for common WebI errors with detailed explanations, root causes, and solutions.

## 6.1 #MULTIVALUE Error

**Description:** Formula returns multiple values where only one value is expected.

### Root Causes

1. **Missing dimension in block**
   - Formula aggregates data but block lacks dimension needed for proper grouping

2. **Measure used in wrong context**
   - Trying to display measure in Detail section
   - Measure aggregation ambiguous

### Troubleshooting Steps

**Step 1: Identify the problematic variable**
- Note which cell shows #MULTIVALUE
- Check the variable formula

**Step 2: Check block dimensions**
- What dimensions are in the block?
- What dimensions does the formula reference?

**Step 3: Apply appropriate solution**

### Solution Pattern 1: Add Missing Dimension to Block

**Scenario:** Showing max arrest count per officer, but [Officer ID] not in block.

```webi
// Variable: var_Max_Arrests_Per_Officer
// Qualification: Measure
Max([Arrests])

// Block contains only: [District]
// Result: #MULTIVALUE (WebI doesn't know which officer's max to show)

// SOLUTION: Add [Officer ID] to block
// Block now contains: [District], [Officer ID]
// Result: Shows correctly
```

### Solution Pattern 2: Use Context Operators

**Scenario:** Want to show district-level max, but officer is in block.

```webi
// PROBLEM: This gives #MULTIVALUE because [Officer ID] is in block
// Variable: var_Max_Arrests_By_District
Max([Arrests])

// SOLUTION 1: Use IN operator to specify exact context
Max([Arrests]) In ([District])

// SOLUTION 2: Use ForAll to remove officer from context
Max([Arrests]) ForAll([Officer ID])
```

### Solution Pattern 3: Change Variable Qualification

**Scenario:** Variable returning single value but qualified as Measure.

```webi
// Variable: var_Latest_Case_Date
// Current Qualification: Measure
// Formula: Max([Incident Date]) In ([Case ID])
// Result: May cause #MULTIVALUE in some blocks

// SOLUTION: Change qualification to Detail
// Qualification: Detail
// Explanation: This formula returns one date per Case ID, not an aggregate
```

### Police Data Examples

**Example 1: Case Status by District**

```webi
// Block structure:
[District] | [Status] | [Case Count]

// Want to add: Latest case date for the district
// Variable: var_Latest_District_Case_Date
Max([Case Date])  // This will cause #MULTIVALUE!

// Why? Block has [Status] dimension, so WebI doesn't know if you want:
// - Latest date across all statuses in district?
// - Latest date per status in district?

// SOLUTION: Be explicit
// Latest across all statuses:
Max([Case Date]) ForAll([Status])

// OR latest per status (no change needed, just clarify intent):
Max([Case Date])  // This actually works if that's what you want
```

**Example 2: Officer Performance Summary**

```webi
// Block structure:
[Officer Name] | [Total Cases] | [Avg Response Time]

// Want to add: Department average response time for comparison
// Variable: var_Dept_Avg_Response_Time
Average([Response Time])  // #MULTIVALUE!

// Why? Formula tries to aggregate across all officers, but block is per-officer

// SOLUTION: Remove officer from context
Average([Response Time]) ForAll([Officer Name])
```

### Diagnostic Checklist

- [ ] Is the variable qualification correct? (Dimension/Measure/Detail)
- [ ] Are all necessary dimensions in the block?
- [ ] Does the formula need context operators (In, ForEach, ForAll)?
- [ ] Is this formula trying to show detail-level data in an aggregate context?

---

## 6.2 #COMPUTATION Error

**Description:** WebI cannot determine the correct calculation context for the formula.

### Root Causes

1. **Ambiguous aggregation context**
   - Formula doesn't specify which dimensions to aggregate on
   - Required dimensions missing from query or block

2. **Merged dimension issues**
   - Formula references dimensions from multiple queries
   - Incompatible contexts

3. **Complex calculation without proper context**
   - Nested aggregations without explicit context

### Troubleshooting Steps

**Step 1: Check if required dimensions exist**
- Edit Query → Verify all dimensions referenced in formula are in the query

**Step 2: Verify dimensions in block**
- Does the block contain the dimensions needed for calculation context?

**Step 3: Make context explicit**
- Use IN operator to specify exact dimensions

### Solution Pattern 1: Specify Context with IN

**Scenario:** Calculation fails because context is ambiguous.

```webi
// PROBLEM: This may cause #COMPUTATION
// Variable: var_Max_Monthly_Arrests
Max([Arrests])

// If block doesn't have [Month], or has additional dimensions, WebI is confused

// SOLUTION: Explicitly specify dimensions
Max([Arrests]) In ([Officer ID]; [Month])

// Now WebI knows: "Calculate max arrests for each Officer-Month combination"
```

### Solution Pattern 2: Add Missing Dimension with ForEach

**Scenario:** Dimension exists in query but not in block, causing #COMPUTATION.

```webi
// Query has: [District], [Officer ID], [Month], [Arrests]
// Block has: [Officer ID], [Arrests]
// Variable: var_District_Total
Sum([Arrests])  // #COMPUTATION!

// Why? WebI doesn't know how to handle [District] (in query but not in block)

// SOLUTION: Tell WebI to include District in calculation
Sum([Arrests]) ForEach([District])
```

### Solution Pattern 3: Fix Merged Dimension Issues

**Scenario:** Formula references dimensions from multiple queries without proper merge.

```webi
// Query 1: Case data with [Case ID], [District], [Status]
// Query 2: Officer data with [Officer ID], [District], [Badge Number]

// Variable tries to combine data:
Sum([Cases]) + Count([Officers])  // #COMPUTATION!

// Why? [District] from Query 1 and [District] from Query 2 are not merged

// SOLUTION:
// 1. Right-click [District] from Query 1
// 2. Select "Merge"
// 3. Choose [District] from Query 2
// 4. Now formula works with merged dimension
```

### Police Data Examples

**Example 1: District Response Time Analysis**

```webi
// Query has: [District], [Priority Level], [Response Time Minutes]
// Block structure: [District] | [Avg Response Time]

// Want to add: Maximum response time for high-priority calls only
// Variable: var_Max_High_Priority_Response
Max([Response Time Minutes]) Where ([Priority Level] >= 7)  // #COMPUTATION!

// Why? [Priority Level] is in query but not in block, and WebI is confused

// SOLUTION 1: Add Priority Level to block (changes report structure)
// SOLUTION 2: Use explicit context
Max([Response Time Minutes]) Where ([Priority Level] >= 7) In ([District])

// OR if Priority Level should be considered:
Max([Response Time Minutes]) Where ([Priority Level] >= 7) In ([District]; [Priority Level])
```

**Example 2: Cross-Query Officer Case Count**

```webi
// Query 1: Cases with [Case ID], [Officer ID], [Status]
// Query 2: Officer details with [Officer ID], [Name], [Badge Number]
// [Officer ID] is MERGED

// Block structure: [Officer Name] | [Total Cases]

// Variable: var_Open_Cases_Per_Officer
Count([Case ID]) Where ([Status] = "Open")

// This works IF:
// 1. [Officer ID] is properly merged
// 2. Block includes merged [Officer ID] or dimension from same query as [Officer Name]

// If #COMPUTATION occurs:
// - Verify merge is correct
// - Use explicit context: Count([Case ID]) Where ([Status] = "Open") In ([Officer ID])
```

### Diagnostic Checklist

- [ ] Are all dimensions in the formula also in the query?
- [ ] Does the block contain sufficient dimensions for context?
- [ ] Are merged dimensions properly configured?
- [ ] Does the formula use explicit context operators (IN)?
- [ ] Are there nested aggregations that need clarification?

---

## 6.3 #DIV/0 Error

**Description:** Division by zero in a formula.

### Root Causes

1. **Denominator is zero**
   - Aggregation results in zero
   - Data naturally contains zeros

2. **Null values treated as zero**
   - Null in denominator
   - Empty aggregation

### Solution Pattern: Always Check Denominator

```webi
// UNSAFE: This will error when denominator is zero
[Numerator] / [Denominator]

// SAFE: Check before dividing
If [Denominator] = 0 Then 0 Else [Numerator] / [Denominator]

// SAFER: Also handle null
If IsNull([Denominator]) Or [Denominator] = 0
Then 0
Else [Numerator] / [Denominator]

// WITH CUSTOM HANDLING: Return null or different value
If IsNull([Denominator]) Or [Denominator] = 0
Then Null
Else [Numerator] / [Denominator]
```

### Police Data Examples

**Example 1: Case Clearance Rate**

```webi
// Variable: var_Clearance_Rate
// Formula: Cases Closed / Total Cases
// Problem: New district with zero cases → #DIV/0

// SOLUTION:
If [Total Cases] = 0 Or IsNull([Total Cases])
Then 0  // Or Null, or "N/A" depending on requirements
Else ([Cases Closed] / [Total Cases]) * 100
```

**Example 2: Average Response Time by District**

```webi
// Variable: var_Avg_Response_Per_Call
// Formula: Total Response Minutes / Call Count
// Problem: District with no calls in time period → #DIV/0

// SOLUTION:
If IsNull([Call Count]) Or [Call Count] = 0
Then 0
Else [Total Response Minutes] / [Call Count]

// ALTERNATIVE: Return null to distinguish "no data" from "zero average"
If IsNull([Call Count]) Or [Call Count] = 0
Then Null
Else [Total Response Minutes] / [Call Count]
```

**Example 3: Percentage Change Calculation**

```webi
// Variable: var_Month_Over_Month_Change_Pct
// Formula: ((Current - Previous) / Previous) * 100
// Problem: Previous month had zero arrests → #DIV/0

// SOLUTION:
If IsNull(Previous([Arrests])) Or Previous([Arrests]) = 0
Then 0  // Or Null, or handle specially
Else (([Arrests] - Previous([Arrests])) / Previous([Arrests])) * 100

// ALTERNATIVE: Show different message
If IsNull(Previous([Arrests])) Or Previous([Arrests]) = 0
Then "N/A - No Previous Data"
Else FormatNumber((([Arrests] - Previous([Arrests])) / Previous([Arrests])) * 100, "0.00") + "%"
```

### Best Practices

1. **Always check denominator before division**
2. **Decide on appropriate default:** 0, Null, or text message
3. **Handle null explicitly** with `IsNull()` check
4. **Document the logic** - why did you choose 0 vs Null?

---

## 6.4 #INCOMPATIBLE Error

**Description:** Trying to combine incompatible dimensions from different data providers.

### Root Causes

1. **Unmerged dimensions from different queries**
   - Formula combines dimensions that aren't merged
   - Block contains dimensions from multiple queries without merge

2. **Incompatible context**
   - Dimensions exist at different grain levels
   - Relationship not properly defined

### Solution Pattern 1: Merge Dimensions

**Process:**
1. Identify the common dimension between queries
2. Right-click dimension in one query
3. Select "Merge"
4. Choose matching dimension from other query
5. Merged dimension shown with special icon

**Example:**
```webi
// Query 1: Cases with [Case ID], [District], [Case Type]
// Query 2: District info with [District], [District Name], [Population]

// To use both in same block:
// 1. Right-click [District] from Query 1
// 2. Merge with [District] from Query 2
// 3. Now you can create blocks with both queries' data
```

### Solution Pattern 2: Separate Blocks

**Scenario:** Dimensions cannot be meaningfully merged.

```webi
// Query 1: Incident-level data (1 row per incident)
// Query 2: Officer-level data (1 row per officer)
// No common dimension

// CANNOT merge into single block
// SOLUTION: Use separate blocks or separate reports
```

### Police Data Examples

**Example 1: Cases by District with District Demographics**

```webi
// Query 1: Q_Cases
// - [District] (e.g., "North", "South", "East", "West")
// - [Case Count]

// Query 2: Q_District_Info
// - [District Code] (e.g., "N", "S", "E", "W")
// - [District Population]
// - [Officer Count]

// PROBLEM: [District] and [District Code] are different but represent same thing

// SOLUTION 1: Fix at query level (best)
// - Modify query to use same dimension name/values

// SOLUTION 2: Merge with caution
// - Merge dimensions
// - Verify data aligns correctly

// SOLUTION 3: Use separate blocks
// - One block for cases by district
// - Another block for district info
```

**Example 2: Incident Data with Officer Data**

```webi
// Query 1: Incidents with [Incident ID], [Assigned Officer ID], [Date]
// Query 2: Officers with [Officer ID], [Officer Name], [Rank]

// Want block showing: [Officer Name], [Incident Count]

// STEP 1: Merge [Assigned Officer ID] from Q1 with [Officer ID] from Q2
// Right-click [Assigned Officer ID] → Merge → Select [Officer ID]

// STEP 2: Create block
[Officer Name]  // from Q2
[Incident ID]  // from Q1, shows count using merged dimension

// This works because merge creates relationship
```

### Diagnostic Checklist

- [ ] Are you combining data from multiple queries?
- [ ] Is there a common dimension between the queries?
- [ ] Have you merged the common dimensions?
- [ ] Are the merged dimensions at the same grain (level of detail)?
- [ ] Does the data actually align correctly between queries?

---

## 6.5 #SYNTAX Error

**Description:** Formula contains syntax errors - typos, incorrect function names, or malformed expressions.

### Common Syntax Errors

### Error 1: Misspelled Function Names

```webi
// WRONG:
Summ([Arrests])  // Extra 'm'
Counta([Case ID])  // Should be Count
Avg([Response Time])  // Should be Average

// CORRECT:
Sum([Arrests])
Count([Case ID])
Average([Response Time])
```

### Error 2: Missing Parentheses or Brackets

```webi
// WRONG:
If [Status] = "Open" Then "Active"  // Missing Else clause
Sum([Arrests] Where ([District] = "North")  // Missing closing paren

// CORRECT:
If [Status] = "Open" Then "Active" Else "Closed"
Sum([Arrests]) Where ([District] = "North")
```

### Error 3: Incorrect String Delimiters

```webi
// WRONG:
If [District] = 'North' Then "Yes"  // Mixed quotes
If [Status] = North Then "Open"  // Missing quotes

// CORRECT:
If [District] = "North" Then "Yes"
If [Status] = "North" Then "Open"
```

### Error 4: Wrong Operator Syntax

```webi
// WRONG:
If [Priority] == 5  // Wrong equality operator
If [Cases] > 100 && [Status] = "Open"  // Wrong AND operator
[First Name] & " " & [Last Name]  // Wrong concatenation

// CORRECT:
If [Priority] = 5
If [Cases] > 100 And [Status] = "Open"
[First Name] + " " + [Last Name]
```

### Error 5: Incorrect Context Operator Syntax

```webi
// WRONG:
Max([Arrests]) In [District]  // Missing parentheses
Sum([Cases]) ForAll [Status]  // Missing parentheses
Average([Response Time]) Where [Priority] = 5  // Missing parentheses on right side

// CORRECT:
Max([Arrests]) In ([District])
Sum([Cases]) ForAll ([Status])
Average([Response Time]) Where ([Priority] = 5)
```

### Error 6: Date Format Errors

```webi
// WRONG:
ToDate("2024/01/15", "MM-DD-YYYY")  // Format doesn't match string
FormatDate([Date], "DD-MM-YY")  // Lowercase 'yy' should be uppercase

// CORRECT:
ToDate("2024/01/15", "yyyy/MM/dd")
FormatDate([Date], "dd-MM-yyyy")
```

### Police Data Examples

**Example 1: Officer Name Concatenation**

```webi
// WRONG:
[Officer Last Name] & ", " & [Officer First Name]  // Wrong operator

// CORRECT:
[Officer Last Name] + ", " + [Officer First Name]
```

**Example 2: Case Priority Logic**

```webi
// WRONG:
If [Priority] >= 7 Then "High"  // Missing Else
If [Priority] >= 7 Then "High" ElseIf [Priority] >= 4 Then "Medium"  // Missing final Else

// CORRECT:
If [Priority] >= 7 Then "High" Else "Normal"
If [Priority] >= 7 Then "High" ElseIf [Priority] >= 4 Then "Medium" Else "Low"
```

**Example 3: Date Filtering**

```webi
// WRONG:
[Incident Date] > CurrentDate - 30  // Wrong syntax for date math

// CORRECT:
[Incident Date] >= RelativeDate(CurrentDate(); -30)
```

### Debugging Tips

1. **Copy formula to text editor** - easier to spot syntax errors
2. **Check each function name** against WebI documentation
3. **Count parentheses and brackets** - ensure they match
4. **Verify string quotes** - use double quotes consistently
5. **Test simple version first** - build complexity gradually

---

## 6.6 #DATASYNC Error

**Description:** Data synchronization error when using merged dimensions.

### Root Causes

1. **Merged dimensions have mismatched data**
   - Values in one query don't exist in other
   - Different data types or formats

2. **Synchronization setting mismatch**
   - "Extend merged dimension values" setting
   - Query synchronization options

### Solution Pattern 1: Check Data Alignment

**Steps:**
1. Create separate blocks for each query
2. Verify dimension values match exactly
3. Check for:
   - Spelling differences
   - Extra spaces
   - Different cases (NORTH vs North)
   - Different data types (string "123" vs number 123)

### Solution Pattern 2: Adjust Synchronization Settings

**Location:** Report Properties → Data Synchronization

**Options:**
- **Extend merged dimension values:** Includes all values from all queries (like outer join)
- **Do not extend:** Only shows matching values (like inner join)

### Police Data Examples

**Example 1: District Mismatch**

```webi
// Query 1: Cases by District
Districts: "North", "South", "East", "West"

// Query 2: District Details
Districts: "NORTH", "SOUTH", "EAST", "WEST"  // ALL CAPS!

// Result: #DATASYNC when trying to merge

// SOLUTION: Fix at query level
// - Modify Query 2 to use Title Case
// - OR add transformation: InitCap([District])
```

**Example 2: Officer ID Format Mismatch**

```webi
// Query 1: Cases with [Officer ID] as NUMBER (1247, 1248, etc.)
// Query 2: Officer Info with [Officer ID] as STRING ("1247", "1248", etc.)

// Result: #DATASYNC or no matching data

// SOLUTION: Convert to same type at query level
// Query 1: Use ToText([Officer ID])
// OR Query 2: Use ToNumber([Officer ID])
```

### Diagnostic Checklist

- [ ] Do dimension values match exactly between queries?
- [ ] Are data types the same?
- [ ] Are there leading/trailing spaces?
- [ ] Is capitalization consistent?
- [ ] Have you checked synchronization settings?

---

## 6.7 Null Value Issues

**Description:** Unexpected results or errors due to null values in data.

### Common Null Problems

### Problem 1: Nulls in Calculations

```webi
// Expression with null:
[Field1] + [Field2]  // If either is null, result is null

// SOLUTION: Handle nulls explicitly
If IsNull([Field1]) Then 0 Else [Field1] +
If IsNull([Field2]) Then 0 Else [Field2]

// OR more concise:
If IsNull([Field1]) Then [Field2] Else If IsNull([Field2]) Then [Field1] Else [Field1] + [Field2]
```

### Problem 2: Nulls in Comparisons

```webi
// This doesn't match null values:
If [Status] = "Open" Then "Active" Else "Inactive"
// Null statuses become "Inactive" - might not be desired

// SOLUTION: Handle null explicitly
If IsNull([Status]) Then "Unknown"
ElseIf [Status] = "Open" Then "Active"
Else "Inactive"
```

### Problem 3: Nulls in Aggregations

```webi
// Count includes nulls differently:
Count([Officer Name])  // Excludes nulls
Count([Officer Name]; All)  // Includes nulls

// Average ignores nulls:
Average([Response Time])  // Only averages non-null values
```

### Police Data Examples

**Example 1: Case Assignment Status**

```webi
// Variable: var_Assignment_Status
// Data: [Assigned Officer] can be null for unassigned cases

// PROBLEM: This doesn't identify unassigned cases
If [Assigned Officer] = "None" Then "Unassigned" Else "Assigned"

// SOLUTION: Check for null
If IsNull([Assigned Officer]) Then "Unassigned" Else "Assigned"
```

**Example 2: Response Time Calculation with Nulls**

```webi
// Variable: var_Response_Time_Minutes
// Data: [Arrival Time] is null if officers haven't arrived yet

// PROBLEM: This gives null for in-progress calls
([Arrival Time] - [Dispatch Time]) * 24 * 60

// SOLUTION: Handle null explicitly
If IsNull([Arrival Time])
Then "In Progress"
Else FormatNumber(([Arrival Time] - [Dispatch Time]) * 24 * 60, "#,##0") + " min"
```

**Example 3: Data Completeness Check**

```webi
// Variable: var_Missing_Critical_Data
// Check for any null critical fields

// Formula:
If IsNull([Location]) Then "Missing Location; " Else "" +
If IsNull([Incident Type]) Then "Missing Type; " Else "" +
If IsNull([Priority Level]) Then "Missing Priority; " Else ""

// Result: Lists all missing fields, or empty string if complete
```

### Best Practices

1. **Always consider null possibility** in formulas
2. **Use IsNull() function** to explicitly check
3. **Decide on null handling strategy:** Convert to 0, default text, or preserve null
4. **Document null behavior** in variable descriptions

---

## 6.8 Performance Issues

**Description:** Report runs slowly, times out, or crashes.

### Diagnostic Questions

1. **How much data?** Rows in result set
2. **How many queries?** Multiple queries slower
3. **Are filters at query or report level?** Query is faster
4. **How many formulas?** Complex calculations slow things down
5. **Are you merging dimensions?** Merging is expensive

### Solution Pattern 1: Move Filters to Query Level

```webi
// SLOW: Report-level filter
// Report Filter: [District] = "North"
// Problem: Fetches all districts, then filters in WebI

// FAST: Query-level filter
// Edit Query → Query Filters → [District] = "North"
// Problem solved: Only fetches North district from database

// Performance improvement: 10-100x faster for large datasets
```

### Solution Pattern 2: Optimize Scope of Analysis

```webi
// SLOW: Full scope of analysis
// Query Properties → Scope of Analysis → "All dimensions"
// Problem: Fetches extra data for drilling that may not be needed

// FAST: Limited scope
// Query Properties → Scope of Analysis → "None" (if no drilling needed)
// OR "One Level" (if minimal drilling needed)

// Performance improvement: 20-50% faster, smaller result set
```

### Solution Pattern 3: Remove Unused Objects

**Process:**
1. Edit Query
2. Remove any objects not used in report
3. Minimize number of dimensions
4. Only include needed measures

```webi
// BEFORE: Query has 50 columns, report uses 10
// AFTER: Query has 10 columns, report uses 10
// Performance improvement: Faster query, less memory
```

### Solution Pattern 4: Simplify Complex Formulas

```webi
// SLOW: Nested complex formula calculated in WebI
If [Field1] > (Average([Field2]) * 1.5) And [Field3] In (
  SELECT DISTINCT...
)

// FASTER: Move complexity to database
// Create database view or modify universe
// Pull pre-calculated flags or simplified values
```

### Solution Pattern 5: Limit Merged Dimensions

```webi
// SLOW: 5 merged dimensions across 3 queries
// Problem: Merging requires expensive join operations in WebI

// FASTER: Minimize merges
// - Combine queries at database level if possible
// - Use single query with proper joins
// - Only merge when absolutely necessary
```

### Police Data Examples

**Example 1: District Crime Report**

```webi
// SLOW VERSION:
// - Query fetches all 100,000 incidents from last year
// - Report filter: [Incident Date] >= RelativeDate(CurrentDate(); -30)
// - Report filter: [District] = "North"
// Problem: Fetches 100,000 rows, filters to 2,000 in WebI

// FAST VERSION:
// - Query filters:
//   - [Incident Date] >= RelativeDate(CurrentDate(); -30)
//   - [District] = "North"
// - No report filters needed
// Problem solved: Fetches only 2,000 rows from database

// Performance: 50x faster
```

**Example 2: Officer Performance Dashboard**

```webi
// SLOW VERSION:
// - 3 separate queries: Cases, Officers, Districts
// - Merged dimensions: [Officer ID], [District]
// - 50 complex variables with nested aggregations
// - Full scope of analysis on all dimensions

// FAST VERSION:
// - 1 query with proper joins at database level
// - Minimal variables, complex calculations done in database
// - Scope of analysis: "None" (no drilling needed)
// - Query filters for date range

// Performance: 10x faster, more stable
```

### Performance Troubleshooting Checklist

- [ ] Are filters at query level (not report level)?
- [ ] Is scope of analysis minimized?
- [ ] Have you removed unused query objects?
- [ ] Are merged dimensions minimized?
- [ ] Are complex calculations really necessary in WebI?
- [ ] Is the database properly indexed?
- [ ] Could you use an aggregated table instead?
- [ ] Have you tested with smaller date range first?

---

## 6.9 General Troubleshooting Workflow

**Step-by-Step Process for Any WebI Error:**

### Step 1: Identify the Error

- Note exact error message (#MULTIVALUE, #DIV/0, etc.)
- Identify which cell/variable shows the error
- Check if error appears in all blocks or specific ones

### Step 2: Isolate the Problem

**Test the variable alone:**
1. Create new simple block
2. Add just one dimension
3. Add the problematic variable
4. Does error still occur?

**Test components:**
1. If variable uses other variables, test those individually
2. Simplify formula - remove parts until it works
3. Identify which part causes the error

### Step 3: Check Context

**For aggregation errors (#MULTIVALUE, #COMPUTATION):**
- [ ] What dimensions are in the block?
- [ ] What dimensions does the formula reference?
- [ ] Is there a mismatch?

**For data errors (#DATASYNC, #INCOMPATIBLE):**
- [ ] Are you using multiple queries?
- [ ] Are dimensions merged?
- [ ] Do values match between queries?

### Step 4: Apply Solution

Based on error type, apply appropriate solution from sections 6.1-6.8.

### Step 5: Test Thoroughly

- Test in multiple blocks
- Test with different filters
- Test edge cases (nulls, zeros, empty results)
- Test with different date ranges

### Step 6: Document

Add comments to variable formulas explaining:
- What the formula does
- Why specific handling is needed (e.g., "Check for null to avoid #DIV/0")
- Any assumptions or limitations

---

## 6.10 Error Prevention Best Practices

### Practice 1: Always Handle Edge Cases

```webi
// When writing formulas, always consider:
// - What if denominator is zero?
// - What if field is null?
// - What if no data matches filter?
// - What if date range returns empty results?

// EXAMPLE: Safe percentage calculation
If IsNull([Total]) Or [Total] = 0
Then 0
Else If IsNull([Count]) Then 0 Else ([Count] / [Total]) * 100
```

### Practice 2: Use Explicit Context

```webi
// Instead of relying on default context:
Max([Revenue])  // Ambiguous

// Always specify:
Max([Revenue]) In ([District]; [Month])  // Clear intent
```

### Practice 3: Test Incrementally

```webi
// Don't write complex formulas all at once:
If IsNull([Field1]) Or [Field1] = 0 Then 0 Else (([Field2] * [Field3]) / [Field1]) * 100 + [Field4]

// Build step by step:
// Step 1: Test [Field2] * [Field3]
// Step 2: Test division with null/zero check
// Step 3: Add percentage conversion
// Step 4: Add [Field4]
```

### Practice 4: Use Helper Variables

```webi
// Instead of one massive formula:
// Variable: var_Complex_Result
If IsNull([A]) Or [A] = 0 Then 0 Else (([B] + [C] - [D]) / [A]) * 100

// Break into helper variables:
// Variable: var_Sum_BCD
[B] + [C] - [D]

// Variable: var_Safe_Ratio
If IsNull([A]) Or [A] = 0 Then 0 Else [var_Sum_BCD] / [A]

// Variable: var_Complex_Result
[var_Safe_Ratio] * 100

// Easier to debug, test, and maintain
```

### Practice 5: Document Assumptions

```webi
// Variable: var_Case_Clearance_Rate
// Formula: ([Cases Closed] / [Total Cases]) * 100
// NOTES:
// - Returns 0 if Total Cases is 0 or null (new districts)
// - Only counts cases with Status = "Closed" or "Cleared"
// - Excludes transferred cases (counted in receiving district)
```

---

# Part 7: Optimization Guide

This section provides comprehensive performance optimization strategies for WebI reports, organized from highest to lowest impact.

## 7.1 Query-Level Optimization (Highest Impact)

Query-level optimizations have the biggest performance impact (10-100x improvement) because they reduce data retrieved from the database.

### 7.1.1 Use Query Filters Instead of Report Filters

**Impact:** 10-100x performance improvement

**Rule:** If filter criteria are known before report runs, use query filters.

```webi
// SLOW: Report filter
// Fetches 100,000 incidents, filters to 2,000 in WebI
// Report Filter: [District] = "North"

// FAST: Query filter
// Fetches only 2,000 incidents from database
// Edit Query → Query Filters → [District] = "North"
```

**When to Use Query Filters:**
- Fixed geographic regions (district, precinct)
- Fixed time periods (last 30 days, current year)
- Fixed status values (Active, Open, Closed)
- Any filter that doesn't need to change between refreshes

**When to Use Report Filters:**
- Interactive filtering by end users
- Dynamic filtering based on user selection
- Filtering that changes frequently without rerunning query

### 7.1.2 Limit Date Ranges in Query

**Impact:** 50-90% reduction in data volume

```webi
// SLOW: No date filter - fetches all historical data
// Query fetches: 500,000 incidents from 2010-present

// FAST: Add date filter
// Query Filter: [Incident Date] >= RelativeDate(CurrentDate(); -365)
// Query fetches: 50,000 incidents from last year only

// Performance: 10x faster
```

**Best Practices:**
- Always include date range filters for transactional data
- Default to reasonable time window (30, 90, or 365 days)
- Use prompts for user-selectable date ranges
- Consider data archiving for very old data

### 7.1.3 Remove Unused Objects from Query

**Impact:** 20-40% performance improvement

**Process:**
1. Edit Query
2. Review all Result Objects
3. Remove any not used in report
4. Minimize dimensions (keep only what's needed for breaks/grouping)
5. Minimize measures (keep only what's displayed or calculated)

```webi
// BEFORE: Query has 50 objects
- [Incident ID]
- [Case ID]
- [Incident Date]
- [Modified Date]
- [Created Date]
- [Incident Type]
- [Incident SubType]
- [Status]
- [Priority]
- [District]
- [Beat]
- [Location Address]
- [Location Latitude]
- [Location Longitude]
... (36 more fields not used)

// AFTER: Query has 8 objects
- [Incident Date]
- [Incident Type]
- [Status]
- [District]
- [Priority Level]
- [Officer ID]
- [Response Time]
- [Case Count]

// Performance: 30% faster, 60% less memory
```

### 7.1.4 Use Predefined Conditions

**Impact:** Ensures consistent query filters

**Setup:** Universe designer creates predefined conditions at universe level.

**Usage:**
```webi
// Instead of manually creating filter each time:
[Incident Date] >= RelativeDate(CurrentDate(); -30) And [Status] Not In ("Cancelled", "Duplicate")

// Use predefined condition:
"Active Incidents - Last 30 Days"

// Benefits:
// - Consistent across all reports
// - Maintained centrally
// - Often optimized at database level
```

### 7.1.5 Optimize Query SQL

**For Universe Designers:** Ensure efficient SQL generation

**Techniques:**
- Use database indexes on filter fields
- Use indexes on join keys
- Consider aggregate awareness (pre-aggregated tables)
- Use database-specific optimizations
- Review generated SQL (Tools → View SQL)

---

## 7.2 Scope of Analysis Optimization

**Impact:** 20-50% performance improvement

### What is Scope of Analysis?

Scope of Analysis determines what additional data WebI fetches to support drilling in reports.

**Options:**
1. **None:** No drilling - fastest
2. **One Level:** Can drill down one level - moderate
3. **Two Levels:** Can drill down two levels - slower
4. **Custom:** Specify exact drilling dimensions - optimized
5. **All Dimensions:** Can drill anywhere - slowest

### Optimization Strategy

```webi
// SLOW: Full scope of analysis
// Query Properties → Scope of Analysis → "All dimensions"
// Problem: Fetches extra dimensions not shown in report
// Example: Fetches Beat even though report only shows District

// FAST: No scope of analysis
// Query Properties → Scope of Analysis → "None"
// Problem solved: Only fetches dimensions used in report
// Performance: 30% faster, smaller result set

// BALANCED: Custom scope
// Query Properties → Scope of Analysis → "Custom"
// Select only: [District] → [Beat] (if drilling from district to beat)
// Performance: 20% faster than "All", drilling still works
```

### Police Data Example

```webi
// Report shows: District-level incident counts
// User needs: Ability to drill to Beat level

// WRONG: Scope = "All dimensions"
// Fetches: District, Beat, Officer, Shift, Day of Week, Hour, etc.
// Problem: Fetching 10+ dimensions when only need 2

// RIGHT: Scope = "Custom"
// Fetches: District, Beat only
// Result: 40% faster, drilling works as needed
```

---

## 7.3 Formula Optimization

**Impact:** 10-30% performance improvement

### 7.3.1 Use Simple Aggregations When Possible

```webi
// SLOWER: Complex nested aggregation
Sum(If [Status] = "Closed" Then 1 Else 0)

// FASTER: Use Where clause
Sum([Cases]) Where ([Status] = "Closed")

// FASTER STILL: Use Count
Count([Case ID]) Where ([Status] = "Closed")
```

### 7.3.2 Avoid Repeated Calculations

```webi
// SLOW: Same calculation repeated
// Variable: var_Display1
If [Total Cases] = 0 Then 0 Else ([Closed Cases] / [Total Cases]) * 100

// Variable: var_Display2
FormatNumber(If [Total Cases] = 0 Then 0 Else ([Closed Cases] / [Total Cases]) * 100, "0.00")

// Variable: var_Display3
If (If [Total Cases] = 0 Then 0 Else ([Closed Cases] / [Total Cases]) * 100) > 80 Then "Good" Else "Needs Improvement"

// FAST: Calculate once, reuse
// Variable: var_Clearance_Rate_Raw
If [Total Cases] = 0 Then 0 Else ([Closed Cases] / [Total Cases]) * 100

// Variable: var_Clearance_Rate_Display
FormatNumber([var_Clearance_Rate_Raw], "0.00")

// Variable: var_Clearance_Category
If [var_Clearance_Rate_Raw] > 80 Then "Good" Else "Needs Improvement"
```

### 7.3.3 Use Explicit Context Operators

```webi
// SLOWER: WebI must infer context
Max([Response Time])

// FASTER: Context specified explicitly
Max([Response Time]) In ([District]; [Month])

// Why faster? WebI doesn't need to analyze block structure to determine context
```

### 7.3.4 Minimize String Operations

```webi
// SLOW: Complex string manipulation in WebI
Upper(Left([Full Address], Pos([Full Address], ",") - 1))

// FAST: Pre-calculate in database or universe
[Street Name]  // Extracted at database level

// Rule: String operations (Upper, Lower, Substr, Pos, Replace) are slower than numeric operations
```

### 7.3.5 Cache Complex Calculations in Variables

```webi
// SLOW: Complex formula used in multiple places
// Used in 5 different cells:
If IsNull([A]) Or [A] = 0 Then 0 Else (([B] * [C] - [D]) / [A]) * If [E] > 10 Then 1.1 Else 1.0

// FAST: Calculate once in variable
// Variable: var_Complex_Metric
If IsNull([A]) Or [A] = 0 Then 0 Else (([B] * [C] - [D]) / [A]) * If [E] > 10 Then 1.1 Else 1.0

// Then use [var_Complex_Metric] in all 5 cells
// Performance: Calculation happens once, not 5 times per row
```

---

## 7.4 Report Design Optimization

**Impact:** 10-40% performance improvement

### 7.4.1 Minimize Number of Blocks

**Rule:** Each block requires separate processing

```webi
// SLOW: 10 separate blocks for different metrics
Block 1: District | Total Cases
Block 2: District | Open Cases
Block 3: District | Closed Cases
Block 4: District | Avg Response Time
... (6 more blocks)

// FAST: Single block with all metrics
Block 1: District | Total Cases | Open Cases | Closed Cases | Avg Response Time | ...

// Performance: 50% faster (one pass through data instead of 10)
```

**Exception:** Separate blocks needed when dimensions differ significantly.

### 7.4.2 Use Breaks Instead of Multiple Blocks

```webi
// SLOW: Separate block for each district
Block "North": [Officer] | [Cases] WHERE [District] = "North"
Block "South": [Officer] | [Cases] WHERE [District] = "South"
Block "East": [Officer] | [Cases] WHERE [District] = "East"
Block "West": [Officer] | [Cases] WHERE [District] = "West"

// FAST: Single block with break on District
Block 1: [District] | [Officer] | [Cases]
Break on: [District]

// Performance: 4x faster
```

### 7.4.3 Limit Block Rows with Query Filters

```webi
// SLOW: Block with 50,000 rows
// All incidents from last year, no filters

// FAST: Block with 5,000 rows
// Query Filter: [Priority Level] >= 5
// Only high-priority incidents

// Rule: Blocks with >10,000 rows are slow
// Solution: Filter at query level or aggregate data
```

### 7.4.4 Use Appropriate Chart Types

**Performance Ranking (fastest to slowest):**
1. Table (fastest)
2. Bar chart (fast)
3. Line chart (fast)
4. Pie chart (moderate)
5. Scatter plot (slow with many points)
6. Complex visualizations (slowest)

**Optimization:**
```webi
// SLOW: Scatter plot with 50,000 points
// Each point requires rendering

// FAST: Aggregate first, then chart
// Group by hour/district, then scatter plot
// 24 hours × 4 districts = 96 points instead of 50,000
```

---

## 7.5 Data Provider Optimization

**Impact:** 20-60% performance improvement

### 7.5.1 Minimize Number of Data Providers

**Rule:** Each query = separate database hit

```webi
// SLOW: 5 separate queries
Query 1: Cases by District
Query 2: Officers by District
Query 3: Response Times by District
Query 4: District Demographics
Query 5: Historical Comparisons

// FAST: 1-2 queries
Query 1: Combined Case/Officer/Response data (joined at DB level)
Query 2: District Demographics (separate if truly needed)

// Performance: 3x faster
```

**When Multiple Queries Are Acceptable:**
- Different data sources (different databases)
- Significantly different grain (incident-level vs district-level)
- Very different security contexts
- Reusable queries across multiple reports

### 7.5.2 Minimize Merged Dimensions

**Rule:** Merging = expensive join operation in WebI

```webi
// SLOW: 4 merged dimensions across 3 queries
Merged: [District], [Officer ID], [Date], [Incident Type]

// FAST: Join at database level, use 1 query
// No merging needed

// If merging unavoidable: Minimize to 1-2 merged dimensions
```

**Merged Dimension Performance:**
- 1 merged dimension: Acceptable
- 2 merged dimensions: Slower but manageable
- 3+ merged dimensions: Significantly slow
- 5+ merged dimensions: Avoid if possible

### 7.5.3 Use Compatible Dimension Values

**Rule:** Merged dimensions with mismatched values are slower

```webi
// SLOWER: Merged dimensions with partial matches
Query 1: Districts = "North", "South", "East", "West" (100 incidents each)
Query 2: Districts = "North", "South" only (no East/West data)
// WebI must handle unmatched values

// FASTER: Merged dimensions with all matching values
Query 1: Districts = "North", "South"
Query 2: Districts = "North", "South"
// Clean merge, no special handling
```

---

## 7.6 Large Dataset Strategies

**For datasets > 100,000 rows**

### 7.6.1 Use Aggregated Tables

**Database-Level Solution:**

```sql
-- Instead of querying raw incidents table (10M rows):
SELECT District, COUNT(*) as IncidentCount
FROM Incidents
WHERE IncidentDate >= '2024-01-01'
GROUP BY District

-- Use pre-aggregated summary table (100 rows):
SELECT District, IncidentCount
FROM IncidentSummary_Daily
WHERE SummaryDate >= '2024-01-01'
```

**Benefits:**
- 100-1000x faster for aggregate reports
- Reduced load on production database
- Consistent performance regardless of data volume

**Setup:**
- Database team creates aggregate tables
- Universe designer maps to aggregate tables
- WebI automatically uses aggregate when appropriate

### 7.6.2 Implement Data Pagination

**For detail reports with many rows:**

```webi
// Instead of fetching all 100,000 incidents:
// Use Top N or pagination

// Query Filter: Rank([Incident Date], "Descending") <= 1000
// This fetches only latest 1,000 incidents

// OR use prompt for date range:
// @Prompt('Start Date', 'D', {'2024-01-01'}, mono, free)
// User specifies smaller date window
```

### 7.6.3 Use Query Stripping

**For drill-down reports:**

```webi
// Initial query: Aggregate level
Query Filter: None
Result: District-level summary (4 rows)

// When user drills to specific district:
// WebI adds filter automatically
Query Filter: [District] = "North"
Result: Beat-level detail for North only (50 rows instead of 200)
```

**Setup:**
- Enable query stripping in document properties
- Design report with appropriate drill paths
- WebI automatically optimizes queries on drill

---

## 7.7 Universe-Level Optimization

**For Universe Designers**

### 7.7.1 Use Aggregate Awareness

**Setup:** Map high-level objects to aggregate tables

```
// Universe mapping:
[District] → DISTRICT_SUMMARY.District (aggregate table)
[Total Cases] → DISTRICT_SUMMARY.CaseCount (pre-calculated)

// When query uses only [District] and [Total Cases]:
// Uses DISTRICT_SUMMARY (100 rows)

// When query uses [District], [Beat], [Total Cases]:
// Uses CASE_DETAIL table (100,000 rows) - more detail needed
```

**Performance:** 10-100x improvement for aggregate reports

### 7.7.2 Optimize Join Paths

**Rule:** Minimize number of joins

```sql
-- SLOW: 5-table join
Incidents → CaseDetails → Officers → Districts → Divisions

-- FAST: 2-table join (denormalized)
IncidentSummary → Districts
```

### 7.7.3 Index Filter Fields

**Database Optimization:**

```sql
-- Ensure indexes exist on common filter fields:
CREATE INDEX idx_incident_date ON Incidents(IncidentDate);
CREATE INDEX idx_district ON Incidents(District);
CREATE INDEX idx_status ON Incidents(Status);
CREATE INDEX idx_priority ON Incidents(PriorityLevel);

-- Composite index for common filter combinations:
CREATE INDEX idx_district_date ON Incidents(District, IncidentDate);
```

**Impact:** 5-50x improvement for filtered queries

---

## 7.8 Optimization Checklist

**Before Publishing Report:**

### Query Level
- [ ] All fixed filters moved to query level?
- [ ] Date range appropriately limited?
- [ ] Unused objects removed from query?
- [ ] Scope of analysis minimized?
- [ ] Appropriate predefined conditions used?

### Formula Level
- [ ] Complex calculations in variables (not repeated)?
- [ ] Explicit context operators used?
- [ ] String operations minimized?
- [ ] Simple aggregations used when possible?

### Report Design Level
- [ ] Number of blocks minimized?
- [ ] Breaks used instead of multiple blocks?
- [ ] Block row counts reasonable (<10,000)?
- [ ] Appropriate chart types selected?

### Data Provider Level
- [ ] Number of queries minimized (1-2 ideal)?
- [ ] Merged dimensions minimized (<3)?
- [ ] Are joins better done at database level?

### Large Dataset Strategies
- [ ] Using aggregate tables where appropriate?
- [ ] Query stripping enabled for drill reports?
- [ ] Pagination implemented for detail reports?

---

## 7.9 Performance Monitoring

### 7.9.1 Measure Query Time

**Tools → View Query Time:**
- Shows execution time for each query
- Identifies slow queries
- Helps prioritize optimization efforts

**Interpretation:**
- < 5 seconds: Good
- 5-15 seconds: Acceptable
- 15-30 seconds: Needs optimization
- > 30 seconds: Requires immediate attention

### 7.9.2 Monitor Data Volume

**Check query result size:**
- Right-click query → Edit Query → Properties
- View "Number of rows retrieved"

**Guidelines:**
- < 10,000 rows: No performance issues expected
- 10,000-50,000 rows: Monitor performance
- 50,000-100,000 rows: Optimization recommended
- > 100,000 rows: Aggregation or filtering required

### 7.9.3 Use WebI Profiler

**For Advanced Optimization:**

**Central Management Console → Enable Profiling**
- Captures detailed performance metrics
- Shows formula calculation time
- Identifies bottlenecks
- Helps pinpoint exact performance issues

---

## 7.10 Common Performance Anti-Patterns

**Patterns to Avoid:**

### Anti-Pattern 1: Report-Level Filters for Large Data

```webi
// BAD:
// Query: Fetch all 500,000 incidents
// Report Filter: [District] = "North"
// Result: Fetch 500,000, use 50,000

// GOOD:
// Query Filter: [District] = "North"
// Result: Fetch 50,000, use 50,000
```

### Anti-Pattern 2: Too Many Data Providers

```webi
// BAD: 8 separate queries with complex merging
// GOOD: 2-3 queries with proper joins

// Rule: If > 3 queries, reconsider data model
```

### Anti-Pattern 3: No Date Filters on Transactional Data

```webi
// BAD: Query all incidents from 2010-present (10M rows)
// GOOD: Query incidents from last 90 days (50K rows)

// Rule: Always filter transactional data by date
```

### Anti-Pattern 4: Complex String Manipulation

```webi
// BAD: Complex parsing in WebI
Upper(Left([Address], Pos([Address], ",") - 1))

// GOOD: Pre-calculate in database or universe
[Street Name]  // Clean, indexed field
```

### Anti-Pattern 5: Full Scope of Analysis

```webi
// BAD: Scope = "All dimensions" when drilling not needed
// GOOD: Scope = "None" or "Custom" with specific drill path
```

---

## 7.11 Optimization Priority Matrix

**Prioritize optimizations by impact vs effort:**

### High Impact, Low Effort (Do First)
1. Move filters to query level
2. Limit date ranges
3. Remove unused query objects
4. Minimize scope of analysis
5. Use breaks instead of multiple blocks

### High Impact, Medium Effort
1. Reduce number of queries
2. Minimize merged dimensions
3. Create helper variables for repeated calculations
4. Optimize block structure

### High Impact, High Effort
1. Implement aggregate tables (requires DBA)
2. Optimize universe joins (requires universe designer)
3. Add database indexes (requires DBA)
4. Implement query stripping

### Medium Impact, Low Effort
1. Use explicit context operators
2. Simplify formulas
3. Use appropriate chart types
4. Clean up variable dependencies

---

## 7.12 Police Data Optimization Examples

### Example 1: Daily Incident Dashboard

**Original (Slow):**
- 3 queries: Incidents, Officers, Districts
- 2 merged dimensions
- Report filters for date and district
- Full scope of analysis
- 15 separate blocks
- Load time: 45 seconds

**Optimized (Fast):**
- 1 query with joined data
- No merged dimensions
- Query filters for date and district
- Scope = "None" (no drilling)
- 3 blocks with breaks
- Load time: 4 seconds

**Improvements:**
- Moved filters to query level (-70% data)
- Combined queries (-2 database hits)
- Eliminated merging (-30% processing)
- Reduced blocks (-50% rendering)
- **Total improvement: 11x faster**

### Example 2: Officer Performance Report

**Original (Slow):**
- Query: All incidents from 2020-present
- No query filters
- 50 columns in query (using 15)
- Complex formulas repeated multiple times
- Load time: 90 seconds

**Optimized (Fast):**
- Query: Incidents from last 12 months only
- Query filters: Date, Status = "Closed"
- 15 columns in query (exact match)
- Helper variables for repeated calculations
- Load time: 8 seconds

**Improvements:**
- Date filter (-95% data volume)
- Status filter (-40% remaining data)
- Removed unused columns (-30% memory)
- Cached calculations (-20% processing)
- **Total improvement: 11x faster**

### Example 3: District Comparison Report

**Original (Slow):**
- 2 queries with district merged
- Detail-level data aggregated in WebI
- Report filter for date range
- Load time: 60 seconds

**Optimized (Fast):**
- 1 query using aggregate table
- Pre-aggregated at database level
- Query filter for date range
- Load time: 3 seconds

**Improvements:**
- Using aggregate table (-99% data volume)
- Pre-aggregation at DB level (-80% WebI processing)
- Moved filter to query level (-50% network transfer)
- **Total improvement: 20x faster**

---



