# WebI Best Practices - Format Usage Guide

## Working with Each Format

### 1. JSON Format (webi_best_practices.json)

**Load in Python:**
```python
import json

with open('webi_best_practices.json', 'r') as f:
    data = json.load(f)

# Access specific sections
formulas = data['sections']['3_formula_patterns']
errors = data['sections']['8_error_handling']
```

**Extract all conditional formulas:**
```python
patterns = data['sections']['3_formula_patterns']['subsections']['3_1_conditional_logic']['patterns']
for pattern in patterns:
    print(f"{pattern['name']}: {pattern['code']}")
```

**Find formulas by complexity:**
```python
# Get all beginner formulas
formula_index = data['formula_index']
for category, info in formula_index.items():
    for formula in info.get('formulas', []):
        if formula.get('complexity') == 'beginner':
            print(f"{category}: {formula['name']}")
```

**JavaScript/Node.js:**
```javascript
const data = require('./webi_best_practices.json');
const contextOps = data.sections['2_calculation_contexts'].subsections;
Object.entries(contextOps).forEach(([key, subsection]) => {
    console.log(`${subsection.title}`);
});
```

---

### 2. AI-Optimized JSON (webi_ai_optimized.json)

**Use for problem-to-solution mapping:**
```python
import json

with open('webi_ai_optimized.json', 'r') as f:
    data = json.load(f)

# Find solution for a specific problem
problem = "i_need_percentage_of_total"
if problem in data['quick_lookup']['by_problem']:
    sections = data['quick_lookup']['by_problem'][problem]
    print(f"Related sections: {sections}")
```

**Access by difficulty level:**
```python
beginner_concepts = data['quick_lookup']['by_difficulty']['beginner']
print(f"Beginner formulas: {beginner_concepts['formulas']}")
print(f"Beginner concepts: {beginner_concepts['concepts']}")
```

**Look up design patterns:**
```python
patterns = data['design_patterns']
for pattern_key, pattern_info in patterns.items():
    print(f"Pattern: {pattern_info['name']}")
    print(f"Problem: {pattern_info['problem']}")
    print(f"Pattern: {pattern_info['pattern']}")
```

**Get performance tips:**
```python
tips = data['performance_tips']
for level, tip_list in tips.items():
    print(f"\n{level.upper()}:")
    for tip in tip_list:
        print(f"  - {tip}")
```

---

### 3. YAML Format (webi_best_practices.yaml)

**Load in Python:**
```python
import yaml

with open('webi_best_practices.yaml', 'r') as f:
    data = yaml.safe_load(f)

# Access context operators
contexts = data['sections']['context_operators']
for op_name, op_info in contexts.items():
    if op_name != 'title':
        print(f"{op_info['syntax']}: {op_info['purpose']}")
```

**In YAML-based systems:**
```yaml
# Use directly in configuration files
formulas:
  context_operators:
    IN: 
      syntax: "[Measure] In ([Dimension1]; [Dimension2])"
      purpose: "Explicitly specify dimensions"
```

**Ruby usage:**
```ruby
require 'yaml'

data = YAML.load_file('webi_best_practices.yaml')
formulas = data['sections']['common_formulas']
formulas.each do |category, details|
  puts "#{category}: #{details}"
end
```

---

### 4. CSV Format (webi_formula_reference.csv)

**Load in Python (Pandas):**
```python
import pandas as pd

df = pd.read_csv('webi_formula_reference.csv')

# All conditional formulas
conditionals = df[df['Category'] == 'Conditional']
print(conditionals[['Formula Name', 'Example', 'Use Case']])

# All beginner formulas
beginners = df[df['Complexity'] == 'beginner']
print(f"Beginner formulas: {len(beginners)}")

# Search by keyword
search_term = 'date'
results = df[df['Formula Name'].str.lower().contains(search_term, na=False)]
print(results[['Formula Name', 'Syntax']])
```

**Excel/Google Sheets:**
- Import CSV directly
- Use pivot tables for analysis
- Filter by complexity, category, or use case
- Sort by complexity for learning path

**SQL Analysis:**
```sql
CREATE TABLE webi_formulas (
    id INT,
    category VARCHAR(50),
    formula_name VARCHAR(100),
    syntax TEXT,
    example TEXT,
    use_case TEXT,
    complexity VARCHAR(20),
    notes TEXT
);

-- Load CSV into database
LOAD DATA INFILE 'webi_formula_reference.csv' INTO TABLE webi_formulas;

-- Find all date functions
SELECT * FROM webi_formulas WHERE category = 'Date';

-- Count by complexity
SELECT complexity, COUNT(*) FROM webi_formulas GROUP BY complexity;
```

---

### 5. Markdown Format (webi_quick_reference.md)

**Use for:**
- Team documentation wikis
- README files
- Training materials
- Print-friendly reference

**Convert to PDF (command line):**
```bash
# Using pandoc
pandoc webi_quick_reference.md -o webi_reference.pdf

# Using wkhtmltopdf
markdown webi_quick_reference.md | wkhtmltopdf - webi_reference.pdf
```

**Extract tables in Python:**
```python
import pandas as pd

# Read markdown tables
df = pd.read_table('webi_quick_reference.md', sep='|', skipinitialspace=True)
print(df)
```

---

## Common Tasks

### Task 1: Build a Formula Chatbot

```python
import json
import random

# Load AI-optimized data
with open('webi_ai_optimized.json', 'r') as f:
    ai_data = json.load(f)

def find_solution(user_problem):
    problems = ai_data['quick_lookup']['by_problem']

    # Normalize input
    normalized = user_problem.lower().replace(' ', '_')

    if normalized in problems:
        return problems[normalized]

    # Fuzzy matching could go here
    return "Problem not found in database"

# Example usage
result = find_solution("I need percentage of total")
print(f"Related sections: {result}")
```

---

### Task 2: Create Learning Path

```python
import json

with open('webi_ai_optimized.json', 'r') as f:
    data = json.load(f)

def generate_learning_path(level='beginner'):
    difficulty = data['quick_lookup']['by_difficulty'][level]

    path = {
        'level': level,
        'formulas': difficulty['formulas'],
        'concepts': difficulty['concepts'],
        'estimated_hours': {
            'beginner': 8,
            'intermediate': 16,
            'advanced': 24
        }[level]
    }

    return path

# Print learning path
path = generate_learning_path('intermediate')
print(f"Intermediate Path ({path['estimated_hours']} hours):")
for formula in path['formulas'][:5]:
    print(f"  - {formula}")
```

---

### Task 3: Formula Documentation Generator

```python
import json
import csv

def generate_formula_docs(category, output_format='markdown'):
    with open('webi_best_practices.json', 'r') as f:
        data = json.load(f)

    formulas = data['formula_index'][category]['formulas']

    if output_format == 'markdown':
        output = f"# {category.title()} Formulas\n\n"
        for formula in formulas:
            output += f"## {formula['name']}\n"
            output += f"**Syntax:** `{formula['syntax']}`\n"
            output += f"**Example:** `{formula['basic_example']}`\n"
            output += f"**Complexity:** {formula['complexity']}\n\n"
        return output

    elif output_format == 'html':
        output = f"<h1>{category.title()} Formulas</h1>"
        for formula in formulas:
            output += f"<h2>{formula['name']}</h2>"
            output += f"<pre>{formula['syntax']}</pre>"
        return output

# Generate documentation
doc = generate_formula_docs('string', 'markdown')
print(doc)
```

---

### Task 4: Troubleshooting Assistant

```python
import json

with open('webi_best_practices.json', 'r') as f:
    data = json.load(f)

def troubleshoot_error(error_code):
    errors = data['sections']['8_error_handling']['errors']

    for error in errors:
        if error['error_code'] == error_code:
            return {
                'error': error_code,
                'cause': error['cause'],
                'solutions': error['solutions']
            }

    return f"Error {error_code} not found in database"

# Usage
result = troubleshoot_error('#MULTIVALUE')
print(f"Error: {result['error']}")
print(f"Cause: {result['cause']}")
print(f"Solutions:")
for solution in result['solutions']:
    print(f"  - {solution}")
```

---

### Task 5: Performance Analyzer

```python
import json

with open('webi_ai_optimized.json', 'r') as f:
    data = json.load(f)

def analyze_performance(query_type='query_level'):
    tips = data['performance_tips'].get(query_type, [])

    analysis = {
        'type': query_type,
        'total_tips': len(tips),
        'tips': tips,
        'priority': 'high' if query_type == 'query_level' else 'medium'
    }

    return analysis

# Analyze all levels
for level in ['query_level', 'variable_level', 'report_level', 'document_level']:
    analysis = analyze_performance(level)
    print(f"{level.upper()}: {analysis['total_tips']} tips")
```

---

### Task 6: Create Interactive Reference

```python
# Flask web app example
from flask import Flask, jsonify, request
import json

app = Flask(__name__)

with open('webi_ai_optimized.json', 'r') as f:
    webi_data = json.load(f)

@app.route('/api/problem/<problem_id>')
def get_solution(problem_id):
    problem = problem_id.replace('-', '_')
    if problem in webi_data['quick_lookup']['by_problem']:
        return jsonify(webi_data['quick_lookup']['by_problem'][problem])
    return {'error': 'Problem not found'}, 404

@app.route('/api/formulas/<category>')
def get_formulas(category):
    with open('webi_best_practices.json', 'r') as f:
        data = json.load(f)

    if category in data['formula_index']:
        return jsonify(data['formula_index'][category])
    return {'error': 'Category not found'}, 404

@app.route('/api/learn/<level>')
def get_learning_path(level):
    if level in webi_data['quick_lookup']['by_difficulty']:
        return jsonify(webi_data['quick_lookup']['by_difficulty'][level])
    return {'error': 'Level not found'}, 404

if __name__ == '__main__':
    app.run(debug=True)
```

---

### Task 7: Batch Processing

```python
import json
import csv

# Read all formulas and analyze
with open('webi_best_practices.json', 'r') as f:
    data = json.load(f)

# Extract all formulas to CSV
output_rows = []
for category, category_data in data['formula_index'].items():
    for formula in category_data.get('formulas', []):
        output_rows.append({
            'category': category,
            'name': formula.get('name', ''),
            'syntax': formula.get('syntax', ''),
            'example': formula.get('basic_example', ''),
            'complexity': formula.get('complexity', '')
        })

# Write to new CSV
with open('webi_all_formulas_extracted.csv', 'w', newline='') as f:
    writer = csv.DictWriter(f, fieldnames=output_rows[0].keys())
    writer.writeheader()
    writer.writerows(output_rows)

print(f"Extracted {len(output_rows)} formulas to CSV")
```

---

## Integration Examples

### Confluence Wiki Integration
```markdown
{% include_relative webi_quick_reference.md %}
```

### GitHub Pages
```yaml
# _config.yml
exclude: [webi_best_practices.json, webi_ai_optimized.json]
include: [webi_quick_reference.md]
```

### Elasticsearch Indexing
```python
from elasticsearch import Elasticsearch

es = Elasticsearch()

# Index all formulas
with open('webi_best_practices.json', 'r') as f:
    data = json.load(f)

for category, info in data['formula_index'].items():
    for idx, formula in enumerate(info.get('formulas', [])):
        es.index(index='webi-formulas', id=f"{category}-{idx}", body={
            'category': category,
            'name': formula['name'],
            'syntax': formula['syntax'],
            'complexity': formula.get('complexity', '')
        })
```

---

## Recommended Format Selection

| Use Case | Recommended Format |
|----------|-------------------|
| API integration | AI-Optimized JSON |
| Database import | CSV |
| Web app backend | Main JSON |
| Team documentation | Markdown |
| Configuration | YAML |
| Database queries | CSV |
| Mobile app | AI-Optimized JSON |
| Training materials | Markdown + PDF |
| Data analysis | CSV + Pandas |
| Chatbot | AI-Optimized JSON |

---

## Performance Considerations

**File Sizes:**
- JSON: ~44 KB (parsed: ~100 KB in memory)
- AI JSON: ~21 KB
- YAML: ~5.6 KB
- CSV: ~30 KB
- Markdown: ~20 KB

**Load Times (Python):**
```python
import time
import json

files = [
    'webi_best_practices.json',
    'webi_ai_optimized.json',
    'webi_best_practices.yaml'
]

for f in files:
    start = time.time()
    with open(f, 'r') as file:
        data = json.load(file)
    end = time.time()
    print(f"{f}: {(end-start)*1000:.2f}ms")
```

All formats load in <10ms on modern hardware.

---

## Summary

- **JSON**: Best for completeness and integration
- **AI JSON**: Best for semantic search and AI processing  
- **YAML**: Best for readability and configuration
- **CSV**: Best for analysis and database import
- **Markdown**: Best for documentation and sharing

Use multiple formats together for maximum flexibility!
