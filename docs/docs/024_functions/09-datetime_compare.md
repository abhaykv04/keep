---
sidebar_label: datetime_compare
---

# datetime_compare

### Input
t1 - datetime.datetime, t2 - datetime.datetime

### Output
Integer - time difference, in seconds

### Example
```yaml
actions:
- name: trigger-slack
  condition:
  - type: threshold
  # datetime_compare(t1, t2) compares t1-t2 and returns the diff in hours
  #   utcnow() returns the local machine datetime in UTC
  #   to_utc() converts a datetime to UTC
  value: keep.datetime_compare(keep.utcnow(), keep.to_utc("{{ steps.this.results[0][0] }}"))
  compare_to: 1 # hours
  compare_type: gt # greater than
```
