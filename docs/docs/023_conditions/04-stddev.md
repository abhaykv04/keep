---
sidebar_label: stddev
---

# 🎯 Stddev (Standard Deviation)

#### The 'stddev' condition implements standard deviation logic. It takes a list or a list of lists, along with a standard deviation threshold ('compare_to'), and returns all values that are farther away from the mean than the standard deviation.
```yaml
-   type: stddev
    name: OPTIONAL (default "stddev")
    value: REQUIRED. The input of the standard deviation algo.
    pivot_column: OPTIONAL. Integer. If supplied, any item of `value` is threatened as
                            a list, and the `pivot_column` is extracted from any item.
                            For example, if pivot_column is 1, then the second column
                            (zero-based) of every item of the value list is used for
                            the calculation (see example for more details)
    compare_to: REQUIRED. Integer. The standard deviation to compare against.
```
### Example
```yaml
condition:
  - name: stddev-condition
    type: stddev
    value:  "{{ steps.db-step.results }}"
    pivot_column: 2
    compare_to: 1
```
For this example, the output of `db-step` step is a list of rows from the db:

`[(1, 2 ,3), (1, 4, 5), (7, 8, 9)]`

The `pivot_column` is 2, hence the values for the stddev calculation are:
`3`, `5` and `9`.

Next, the sttdev condition calculates the stddev:
```math
standard deviation = sqrt(sum((x - mean)^2) / N)
mean = (3 + 5 + 9) / 3 = 5.666666666666667
```

And the standard deviation (sd) is:
```
standard deviation = sqrt(((3-5.666666666666667)^2 + (5-5.666666666666667)^2 + (9-5.666666666666667)^2) / 3)
                   = sqrt((9.555555555555557 + 0.11111111111111116 + 9.555555555555557) / 3)
                   = sqrt(6.740740740740742)
                   = 2.5961484292674155
```

Therefore, the standard deviation of the dataset [3, 5, 9] is approximately 2.596.

Thus, the values that are more than 1 standard deviation from the mean are 3 and 9, since they are outside the range of 5.666666666666667+-2.5961484292674155 (which is [3.0705, 8.2628]).

### Same example without pivot_column
Notice that we used `pivot_column` since the output db `db-step` was a list of rows.
If the output was just list, we could skip it.

For example, if the output of `db-step` was `(3, 5 ,9)`, we could just use:
```yaml
condition:
  - name: stddev-condition
    type: stddev
    value:  "{{ steps.db-step.results }}"
    compare_to: 1
```
