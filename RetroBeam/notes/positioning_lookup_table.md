# Positioning Lookup Table - CORRECTED

## CRITICAL DISCOVERY: True Baseline = "05:23" (No 1s)

**Key Insight**: Any time containing digit "1" cannot be used as baseline - digit "1" creates positioning anomalies. The true standard must exclude all "1"s.

## Complete 8-Case Lookup Table (REVISED)

### Case Detection Logic:
```
if (time == "11:11") -> Case 8
else if (hour_tens == 1 && hour_ones == 1) -> Case 6  
else if (minute_tens == 1 && minute_ones == 1) -> Case 7
else if (hour_tens == 1) -> Case 2
else if (hour_ones == 1) -> Case 3  
else if (minute_tens == 1) -> Case 4
else if (minute_ones == 1) -> Case 5
else -> Case 1 (Standard - NO 1s)
```

### Position Coordinates (CORRECTED):

| Case | Pattern | Hour_tens | Hour_ones | Colon | Min_tens | Min_ones |
|------|---------|-----------|-----------|-------|----------|----------|
| 1    | Standard| 48        | 135       | 209   | 252      | 340      |
| 2    | 1X:XX   | 48        | 113       | 189   | 231      | 319      |
| 3    | X1:XX   | 70        | 135       | 187   | 230      | 318      |
| 4    | XX:1X   | 70        | 157       | 231   | 252      | 318      |
| 5    | XX:X1   | 70        | 157       | 231   | 274      | 340      |
| 6    | 11:XX   | 70        | 113       | 165   | 208      | 296      |
| 7    | XX:11   | 70        | 179       | 275   | 296      | 340      |
| 8    | 11:11   | 92        | 157       | 231   | 252      | 296      |

## Test Cases for Verification:

1. `05:23` -> Case 1 (Standard)
2. `12:34` -> Case 2 (1X:XX)
3. `01:56` -> Case 3 (X1:XX)  
4. `05:14` -> Case 4 (XX:1X)
5. `05:41` -> Case 5 (XX:X1)
6. `11:43` -> Case 6 (11:XX)
7. `05:11` -> Case 7 (XX:11)
8. `11:11` -> Case 8 (11:11)

## Implementation Notes:
- Case priority matters: Check `11:11` first, then compound cases, then single cases
- All coordinates are absolute x positions for 450px width display
- Y position is constant at 124 for all time digits
- Font family: `@font/imagine_font`, size: 117