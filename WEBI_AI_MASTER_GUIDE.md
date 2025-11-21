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

