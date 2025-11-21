# SAP BusinessObjects Web Intelligence - Quick Reference

## Formula Categories

### Conditional Logic
| Pattern | Code | Example |
|---------|------|---------|
| Simple If | `If cond Then val Else val` | `If [Revenue] > 1000 Then "High" Else "Low"` |
| Nested | Multiple ElseIf statements | Multi-level categorization |
| And Logic | `If cond1 And cond2 Then val` | Check multiple criteria |
| Or Logic | `If cond1 Or cond2 Then val` | Check any criterion |

### Aggregation Functions
| Function | Syntax | Use Case |
|----------|--------|----------|
| Sum | `Sum([Measure])` | Total calculation |
| Count | `Count([Object])` | Record count |
| Count Distinct | `Count([Object]; Distinct)` | Unique records |
| Average | `Average([Measure])` | Mean value |
| Min/Max | `Min([Measure])` / `Max([Measure])` | Extremes |

### Context Operators
| Operator | Syntax | Purpose |
|----------|--------|---------|
| IN | `[Measure] In ([Dim1]; [Dim2])` | Specify exact dimensions |
| ForEach | `[Measure] ForEach([Dim])` | Add dimensions |
| ForAll | `[Measure] ForAll([Dim])` | Remove dimensions |
| Where | `[Measure] Where ([Cond])` | Filter calculation |

### String Functions
| Function | Syntax | Example |
|----------|--------|---------|
| Left | `Left([String]; N)` | `Left([ID]; 3)` → First 3 chars |
| Right | `Right([String]; N)` | `Right([Phone]; 4)` → Last 4 chars |
| Substring | `Substr([String]; Start; Len)` | `Substr([Code]; 5; 3)` → 3 chars from pos 5 |
| Concatenate | `[Str1] + [Str2]` | `[First] + " " + [Last]` |
| Replace | `Replace([Str]; "Old"; "New")` | Remove/change text |
| Upper/Lower | `Upper([Str])` / `Lower([Str])` | Case conversion |
| Position | `Pos([String]; "Search")` | `Pos([Email]; "@")` → @ position |

### Date Functions
| Function | Syntax | Example |
|----------|--------|---------|
| Current Date | `CurrentDate()` | Today's date |
| Year | `Year([Date])` | `Year([OrderDate])` → 2023 |
| Month | `Month([Date])` | `Month([Date])` → 1-12 |
| Day Name | `DayName([Date])` | "Monday", "Tuesday", etc. |
| Relative Date | `RelativeDate([Date]; Days)` | `RelativeDate(CurrentDate(); -30)` |
| Days Between | `DaysBetween([Date1]; [Date2])` | Days elapsed |
| Format Date | `FormatDate([Date]; "Format")` | `FormatDate([Date]; "dd/MM/yyyy")` |

### Formatting
| Function | Syntax | Example |
|----------|--------|---------|
| Format Number | `FormatNumber([Num]; "Format")` | `FormatNumber([Revenue]; "#,##0.00")` |
| Currency | Format with $ | `FormatNumber([Price]; "$#,##0.00")` |
| Percentage | Multiply by 100 + % | `FormatNumber([Margin] * 100; "0.0") + "%"` |

## Error Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| #MULTIVALUE | Multiple values per cell | Add dimension or use ForEach |
| #DIV/0 | Division by zero | `If [Denom] <> 0 Then [Num]/[Denom] Else 0` |
| #COMPUTATION | Missing dimension | Ensure all context dims in block |
| #INCOMPATIBLE | Unsync'd providers | Merge common dimensions |

## Optimization Checklist

- [ ] Use query filters for large datasets
- [ ] Remove unused query objects
- [ ] Set appropriate scope of analysis
- [ ] Use naming conventions (var_ prefix)
- [ ] Add zero-division checks
- [ ] Limit merged dimensions
- [ ] Add comments to complex formulas
- [ ] Test with different data scenarios

## Quick Tips

1. **Always check for null/zero** before division
2. **Use IN operator** when you know exact dimensions
3. **Apply filters at query level** for better performance
4. **Use ForEach for hidden dimensions** in query but not displayed
5. **Reset running sums** with second parameter for new groups
6. **Comment complex formulas** for maintenance
