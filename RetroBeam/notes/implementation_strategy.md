# Implementation Strategy: Lookup Table Testing & Dynamic Positioning

## Phase 1: Positioning Verification (Current Phase)
**Goal**: Verify all 8 positioning cases are accurately mapped before implementing live time.

### Approach:
1. **Keep red reference text** for visual alignment verification
2. **Implement lookup table logic** to detect digit patterns and set positions
3. **Systematically test each case** by changing red reference time
4. **Verify positioning accuracy** for all 8 discovered cases

### Benefits:
- **Validates positioning data** before adding complexity
- **Easier debugging** with visible reference alignment  
- **Systematic verification** of all edge cases
- **Clear separation** of positioning logic from time display logic

## Phase 2: Dynamic Time Implementation (Next Phase)
**Goal**: Replace hardcoded positioning with live time-based positioning.

### Key Insight: Time-Based Property Updates
Just like the progress bar updates its `width` property based on `[SECOND]` expressions, we can potentially update digit `x` positions based on time expressions.

### Potential WFF Approach:
```xml
<!-- Hour tens digit with dynamic positioning -->
<Group name="hour_tens" y="124" width="60" height="100">
  <Transform target="x" value="LOOKUP_EXPRESSION_FOR_HOUR_TENS" />
  <PartText x="0" y="0" width="60" height="100">
    <Text align="CENTER">
      <Font family="@font/imagine_font" size="117" color="#ffffff">
        <Template>[HOUR] / 10</Template>
      </Font>
    </Text>
  </PartText>
</Group>
```

### Transform Expression Strategy:
Create complex conditional expressions that detect digit "1" patterns and return appropriate x coordinates:

```
value="([HOUR] == 11 && [MINUTE] == 11) ? 114 : 
       ([HOUR] == 11) ? 70 :
       ([MINUTE] == 11) ? 92 :
       (([HOUR] / 10) == 1) ? 48 :
       (([HOUR] % 10) == 1) ? 70 :
       (([MINUTE] / 10) == 1) ? 70 :
       (([MINUTE] % 10) == 1) ? 70 :
       70"
```

### Implementation Considerations:
- **Expression Complexity**: WFF conditional expressions can become very complex
- **Performance**: Complex expressions may impact rendering performance
- **Maintainability**: Large conditional expressions are hard to debug
- **Alternative**: Canvas-based approach might be more maintainable for complex logic

## Phase 3: Animation Implementation (Future Phase)
**Goal**: Add beam-up animation effects when digits change.

### Requirements:
- Monitor digit value changes
- Trigger beam animations on transitions
- Coordinate animation timing with minute changes
- Implement beam visual effects (expanding/contracting rectangles)

## Current Action Plan:

### Step 1: Implement Test Harness
Create a system to easily test all 8 cases by updating the red reference template and corresponding white digit positions.

### Step 2: Systematic Verification
Test each case in sequence:
1. `05:23` (Standard)
2. `12:34` (1X:XX) 
3. `01:56` (X1:XX)
4. `05:14` (XX:1X)
5. `05:41` (XX:X1)
6. `11:43` (11:XX)
7. `05:11` (XX:11)
8. `11:11` (11:11)

### Step 3: Document Results
Record any discrepancies and refine positioning data.

### Step 4: Implement Dynamic Positioning
Once positioning is verified, implement time-based Transform expressions or switch to Canvas approach based on WFF limitations discovered during testing.