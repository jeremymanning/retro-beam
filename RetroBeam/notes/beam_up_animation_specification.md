# Beam-Up Animation Specification

## Overview
The "beam-up" animation creates a sci-fi effect where digits are carried away by tractor beams and replaced with new digits arriving from above.

## Timing Sequence (per minute cycle)

### Phase 1: Beam Growth (0.175s)
**59.475s → 59.65s** (0.175s duration)
- Beam grows from top of screen down to bottom of to-be-changed digit
- Digit remains normal (white color, normal position)

### Phase 2: Digit Lift-off (0.175s) 
**59.65s → 59.825s** (0.175s duration)
- Beam reaches full extent (top of screen to bottom of digit)
- **Digit changes to BLACK color**
- **Digit animates upward** off screen
- **Beam shrinks back** toward top of screen (bottom retracts upward)

### Phase 3: New Beam Growth (0.175s)
**59.825s → 60.0s** (0.175s duration)  
- New beam grows down from top of screen
- Beam extends to bottom of digit position
- No digit visible during this phase

### Phase 4: New Digit Arrival (0.175s)
**60.0s → 60.175s** (0.175s duration)
- **New digit appears** in normal position (white color)
- **Beam retracts** back to top of screen

## Beam Properties

### Dimensions (at full extent)
- **Width**: Same as digit container (90px)
- **Height**: From top of screen (y=0) to bottom of digit (y=224)
- **Color**: White (#ffffff)

### Individual Beam Positions
- **Hour tens beam**: x=33, width=90
- **Hour ones beam**: x=120, width=90  
- **Minute tens beam**: x=237, width=90
- **Minute ones beam**: x=325, width=90

### Beam Behavior
- Each digit has its own independent beam
- Only digits that CHANGE get beams (minimum 1, maximum 4)
- Beams operate in parallel when multiple digits change
- Beam always extends from y=0 (top) to y=224 (bottom of digit)

## Animation Coordination

### Overlap with Progress Bar
- Phase 2 (59.65s-59.825s) overlaps with progress bar reverse animation
- Progress bar animates 450px→0px during 59.65s-60.0s
- Beam animation ends 0.175s into next minute

### Multiple Digit Changes
- Hour change (XX:59→YY:00): 4 beams total
- Minute tens change (X9:XX→X0:XX): 1-2 beams  
- Normal minute ones change: 1 beam
- All beams synchronized to same timing

## Implementation Strategy

### Phase 1: Debug Beam Positioning
1. Create static white rectangles at full beam positions
2. Change digit colors to RED for contrast
3. Verify beam alignment with digit containers
4. Test all 4 beam positions simultaneously

### Phase 2: Implement Animation
1. Use Transform elements like progress bar
2. Animate beam height growth/shrinkage
3. Animate digit position and color changes
4. Coordinate timing with second precision

### Phase 3: Dynamic Beam Selection
1. Detect which digits will change next minute
2. Only show beams for changing digits
3. Handle special cases (hour rollover, etc.)

## Technical Notes

### WFF Animation Approach
- Use PartDraw with Rectangle elements (like progress bar)
- Use Transform elements for height animation
- Use conditional expressions for timing
- Use Variant elements for color changes

### Timing Calculations
- Base timing on [SECOND] and [MILLISECOND] variables
- Use precise decimal timing for smooth animations
- Coordinate with existing progress bar timing

### Beam Growth Animation Pattern
```
height = (current_time >= start_time && current_time <= end_time) ? 
         (progress * max_height) : 
         (before_start ? 0 : max_height)
```

This specification provides the foundation for implementing the signature beam-up effect that was the original goal of the watch face!