---
sidebar_label: diff
---

# diff(iterable)

### Input
An iterable.

### Output
Opposite of [`all`](all) - returns False if all items are identical, else True

### Example
```yaml
actions:
-   name: trigger-slack
    if: "keep.diff({{ steps.db-step.results }})"
    provider:
        type: slack
        config: " {{ providers.slack-demo }} "
        with:
            message: "Items are equal"
```
