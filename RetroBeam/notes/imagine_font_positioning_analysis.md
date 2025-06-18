# Imagine Font Positioning Analysis

## Discovered Patterns

Through systematic testing, we've identified that digit "1" in the Imagine font creates cascading positioning effects that affect all subsequent elements in a time display.

## Complete Lookup Table: 8 Positioning Cases

**CRITICAL DISCOVERY**: Each combination of digit "1" positions creates unique patterns that are NOT mathematically additive. A lookup table approach is essential.

### Case 1: Standard (No "1"s)
**Pattern**: `[0-2-9][0-9]:[0-2-9][0-2-9]` (excluding "1")
**Example**: `02:34`, `05:56`, `08:27`
**Positions**:
- Hour tens: x="70"
- Hour ones: x="135" 
- Colon: x="187"
- Minute tens: x="230"
- Minute ones: x="318"

### Case 2: Hour Tens = "1" 
**Pattern**: `1[0-9]:[0-2-9][0-2-9]` (excluding minute "1"s)
**Example**: `10:43`, `12:34`
**Effect**: Hour positions shift LEFT by 22px
**Positions**:
- Hour tens: x="48" (-22 from standard)
- Hour ones: x="113" (-22 from standard)
- Colon: x="187" (unchanged)
- Minute tens: x="230" (unchanged)  
- Minute ones: x="318" (unchanged)

### Case 3: Hour Ones = "1"
**Pattern**: `[0-2-9]1:[0-2-9][0-2-9]` (excluding minute "1"s)
**Example**: `01:34`, `01:25`, `01:56`
**Effect**: Uses different baseline positioning
**Positions**:
- Hour tens: x="70" (standard)
- Hour ones: x="135" (standard)
- Colon: x="187" (standard)
- Minute tens: x="230" (standard)
- Minute ones: x="318" (standard)

### Case 4: Minute Tens = "1"
**Pattern**: `[0-2-9][0-9]:1[0-2-9]` (excluding hour "1"s and minute ones "1")
**Example**: `05:14`
**Effect**: Progressive RIGHT shifts for elements after hour tens
**Positions**:
- Hour tens: x="70" (unchanged)
- Hour ones: x="157" (+22 from standard)
- Colon: x="231" (+44 from standard)
- Minute tens: x="252" (+22 from standard)
- Minute ones: x="318" (unchanged)

### Case 5: Minute Ones = "1"
**Pattern**: `[0-2-9][0-9]:[0-2-9]1` (excluding hour "1"s and minute tens "1")
**Example**: `05:41`
**Effect**: Extreme RIGHT shifts for elements after hour tens
**Positions**:
- Hour tens: x="70" (unchanged)
- Hour ones: x="157" (+22 from standard)
- Colon: x="231" (+44 from standard) 
- Minute tens: x="274" (+44 from standard)
- Minute ones: x="340" (+22 from standard)

## Key Observations

1. **Digit "1" has narrower width** than other digits in Imagine font
2. **Cascading effects**: "1" affects all subsequent elements, not just adjacent ones
3. **Direction matters**: Hour "1"s shift LEFT, minute "1"s shift subsequent elements RIGHT
4. **Progressive shifts**: Later "1"s create larger cumulative shifts

### Case 6: "11:XX" (Both Hour Digits = "1")
**Pattern**: `11:[0-2-9][0-2-9]` (excluding minute "1"s)
**Example**: `11:43`
**Effect**: Uniform -22px left shift for all elements after hour tens
**Positions**:
- Hour tens: x="70" (unchanged)
- Hour ones: x="113" (-22)
- Colon: x="165" (-22)
- Minute tens: x="208" (-22)
- Minute ones: x="296" (-22)

### Case 7: "XX:11" (Both Minute Digits = "1") 
**Pattern**: `[0-2-9][0-9]:11` (excluding hour "1"s)
**Example**: `05:11`
**Effect**: Progressive right shifts including hour tens
**Positions**:
- Hour tens: x="92" (+22)
- Hour ones: x="179" (+44)
- Colon: x="253" (+66)
- Minute tens: x="274" (+44)
- Minute ones: x="318" (unchanged)

### Case 8: "11:11" (All Digits = "1")
**Pattern**: `11:11`
**Example**: `11:11`
**Effect**: Mixed shifts - hours right, minutes left  
**Positions**:
- Hour tens: x="114" (+44)
- Hour ones: x="157" (+22)
- Colon: x="209" (+22)
- Minute tens: x="230" (unchanged)
- Minute ones: x="274" (-44)

## Lookup Table Implementation Strategy

**Key Insight**: Effects are NOT additive - each compound case creates unique patterns.

**Implementation Approach**:
1. **Pattern Detection**: Identify which case applies based on digit "1" positions
2. **Direct Lookup**: Use pre-calculated positions for each element
3. **No Mathematical Formulas**: Complex cascading effects require explicit mapping

**WFF Implementation**: Conditional positioning logic with hardcoded coordinates for each case.

**Test Coverage Required**: Systematic verification of all 8 cases to ensure accuracy.