---
name: json-utils
description: Parse, validate, and transform JSON data in Python using the standard library and common patterns. Covers loading/dumping, schema validation, handling nested structures, and error handling for malformed input.
---

# JSON Utilities in Python

A guide to working with JSON data in Python using the built-in `json` module and common patterns for scientific data workflows.

## When to Use This Skill

- Parsing JSON API responses or config files
- Validating JSON structure before processing
- Transforming nested JSON into flat structures
- Handling malformed or missing JSON fields safely

## Quick Reference

```python
import json

# Load from string
data = json.loads('{"key": "value"}')

# Load from file
with open("data.json") as f:
    data = json.load(f)

# Dump to string
text = json.dumps(data, indent=2)

# Safe field access with default
value = data.get("missing_key", "default")
```

## Error Handling

```python
try:
    data = json.loads(raw)
except json.JSONDecodeError as e:
    print(f"Invalid JSON at line {e.lineno}: {e.msg}")
```
