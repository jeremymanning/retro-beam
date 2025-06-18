# Beam Up Watch Face Development Notes

## Session Summary
Successfully ported the "Beam Up" watch face from Pebble/Fitbit to Wear OS using Watch Face Format (WFF). Current status: Basic layout complete with time, day, custom font, and animated progress bar.

## Key Technical Learnings

### Watch Face Format (WFF) Architecture
- **WFF is declarative XML-based** - no code required with `android:hasCode="false"`
- **Uses Scene > Group structure** for organizing elements
- **Elements render in XML order** - later elements appear on top (z-order)

### Custom Fonts in WFF
1. **Font file placement**: `/app/src/main/res/font/imagine.ttf`
2. **Font family XML**: Create `/app/src/main/res/font/imagine_font.xml`:
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <font-family xmlns:android="http://schemas.android.com/apk/res/android">
       <font android:fontStyle="normal" android:fontWeight="400" android:font="@font/imagine" />
   </font-family>
   ```
3. **Reference in WFF**: `family="@font/imagine_font"`

### Text Positioning and Sizing
- **TimeText for digital time**: `format="hh:mm" hourFormat="SYNC_TO_DEVICE"`
- **PartText for custom text**: Use Template with Parameters for dynamic content
- **Font sizing**: Direct pixel values (e.g., `size="117"`)
- **Positioning**: x,y coordinates from top-left, y="124" for time, y="246" for day
- **Ambient mode variants**: Use `<Variant mode="AMBIENT">` for different styles

### Drawing Shapes in WFF
**CRITICAL DISCOVERY**: Shapes must be wrapped in proper containers:
```xml
<Group x="0" y="220" width="450" height="10" name="progress_bar_group">
  <PartDraw x="0" y="0" width="450" height="10">
    <Rectangle x="0" y="0" height="10">
      <Fill color="#FFFFFF" />
      <Transform target="width" value="expression" />
    </Rectangle>
  </PartDraw>
</Group>
```

### Dynamic Animations with Transform
- **Transform target="width"** for dynamic width changes
- **Time expressions**: `[SECOND]`, `[MILLISECOND]` for time-based animations
- **Complex expressions**: Conditional logic with `?` operator
- **Progress bar formula**: 
  ```
  ([SECOND] < 59) ? ([SECOND] * 7.5) : 
  ([MILLISECOND] < 650) ? 450 : 
  (450 * (1 - (([MILLISECOND] - 650) / 350.0)))
  ```

## Current Implementation Status

### ✅ Completed Features
1. **Time display**: HH:MM format with Imagine font at y=124
2. **Day display**: "WWW DD" format with Imagine font at y=246  
3. **Custom Imagine font**: Properly embedded and referenced
4. **Seconds progress bar**: 
   - White bar, 10px height, full width
   - Grows linearly with seconds (0-58)
   - Stays full during 59.0-59.65 seconds
   - Smoothly shrinks from 100% to 0% during 59.65-60.0 seconds
5. **Ambient mode support**: Elements hidden appropriately

### 🚧 Next Steps (Beam Up Animation)
- Split time digits into individual elements for animation control
- Implement "beam up" effect when digits change
- Add beam lines/particles for sci-fi aesthetic

## File Structure
```
BeamUpWatchFace/
├── app/src/main/
│   ├── AndroidManifest.xml (hasCode="false")
│   ├── res/
│   │   ├── font/
│   │   │   ├── imagine.ttf
│   │   │   └── imagine_font.xml
│   │   ├── raw/
│   │   │   └── watchface.xml (main WFF definition)
│   │   ├── values/
│   │   │   └── strings.xml
│   │   └── xml/
│   │       └── watch_face_info.xml
```

## Build Commands
```bash
./gradlew build
./gradlew installDebug
```

## Lessons Learned
1. **WFF has specific container requirements** - shapes need PartDraw wrapper
2. **Font embedding requires both TTF and XML family definition**
3. **Transform expressions support complex conditional logic**
4. **Z-order matters** - place elements in XML order for proper layering
5. **Millisecond precision enables smooth animations**

## Key Working Formulas
- **Progress bar width**: `[SECOND] * 7.5` (450px / 60 seconds)
- **Smooth reset**: Conditional expression with millisecond precision
- **Font sizes**: Time=117px, Day=72px for proper visual hierarchy