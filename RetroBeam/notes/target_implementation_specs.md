# Beam Up Watch Face - Target Implementation Specifications

## Visual Layout

### Time Display
- **Font**: "Imagine" custom font (monospace, sci-fi style)
- **Size**: Large (approximately 48pt based on original Pebble implementation)
- **Format**: HH:MM (24-hour or 12-hour based on device setting)
- **Position**: Horizontally centered on screen
- **Vertical Position**: Bottom of time text aligns slightly above vertical center of screen
- **Color**: White (#FFFFFF)
- **Structure**: 4 separate digit objects + 1 colon
  - Hour tens digit (can be blank for hours 1-9 in 12-hour format)
  - Hour ones digit  
  - Colon separator (:)
  - Minute tens digit
  - Minute ones digit

### Progress Bar (Seconds Indicator)
- **Updates**: Every 15 seconds (0%, 25%, 50%, 75%, 100%)
- **Position**: Top edge slightly below vertical center of screen
- **Width**: Progresses from 0% to 100% screen width over 60 seconds
- **Height**: Same thickness as font stroke thickness
- **Color**: White (#FFFFFF)
- **Spacing**: Same distance from bottom of time to top of progress bar as from bottom of progress bar to top of day text

### Day Display
- **Format**: "WWW DD" (e.g., "MON 15", "TUE 03")
- **Font**: Same "Imagine" font as time
- **Size**: Approximately 50% of time font size (~24pt)
- **Position**: Horizontally centered
- **Vertical Position**: Below progress bar with consistent spacing
- **Color**: White (#FFFFFF)

## Animation Specifications

### Beam Up Effect (Triggered on Minute Change)
1. **Phase 1 - Beam Expands Down (0.25s)**
   - White rectangle starts from top of screen
   - Expands vertically downward to bottom of changing digit(s)
   - Rectangle width: Slightly wider than digit width
   - Rectangle position: Centered on changing digit(s)

2. **Phase 2 - Old Digit Removal (0.25s)**
   - Rectangle retracts upward toward top of screen
   - OLD digit moves WITH the rectangle (gets "beamed up")
   - Other digits remain stationary
   - Intersection effect: Where beam overlaps digit, digit appears black instead of white

3. **Phase 3 - New Digit Delivery (0.25s)**
   - Rectangle expands downward again from top of screen
   - NEW digit moves WITH the rectangle (gets "beamed down")
   - Intersection effect: Where beam overlaps digit, digit appears black instead of white

4. **Phase 4 - Beam Retraction (0.25s)**
   - Rectangle retracts upward toward top of screen
   - NEW digit stays in final position
   - Digit returns to white color

### Animation Triggers
- **Minute changes**: Animate minute digits (tens and/or ones as needed)
- **Hour changes**: Animate hour digits (tens and/or ones as needed)
- **Individual digit control**: Each digit can animate independently

## Technical Implementation Notes

### Digit Independence
- Each digit (hour tens, hour ones, minute tens, minute ones) is a separate object
- Each can be positioned and animated independently
- Colon separator is static (never animates)

### Color Inversion Effect
- Normal state: White digits on black background
- During beam intersection: Black digits on white beam background
- This creates the "teleporter" visual effect

### Timing Precision
- Total animation duration: ~1 second (4 phases × 0.25s each)
- Progress bar updates: Every 15 seconds exactly
- Smooth 60fps animation target

### Font Requirements
- Original uses custom "Imagine" font (not available in WFF)
- Fallback: Use system monospace font with similar weight/style
- Maintain proper character spacing and proportions

## Layout Measurements (Based on Original 144×168 Pebble Screen)
- **Time Y position**: ~53px from top (bottom of text ~113px from top)
- **Progress bar Y**: ~105px from top  
- **Day text Y**: ~130px from top
- **Digit spacing**: ~34px between hour/minute pairs
- **Character width**: ~50px per digit
- **Beam width**: ~30px (slightly wider than digit)

## WFF Implementation Strategy
Since WFF doesn't support the complex beam/digit intersection effects, we'll need to:
1. Use separate TimeText elements for each digit
2. Use Transform/Animate for vertical movement
3. Use Group elements for beam rectangles
4. Coordinate timing with Trigger elements
5. Accept limitation: May not achieve exact color inversion effect