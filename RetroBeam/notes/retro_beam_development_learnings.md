# Retro Beam Development Learnings

Date: 2025-06-17
Commit: f5f7b07

## Project Overview
Successfully transformed "Beam Up Pixel" watch face into "Retro Beam" with new features and color scheme for WearOS using Watch Face Format (WFF).

## Key Learnings and Solutions

### 1. WFF Text Positioning in Round Watch Faces
**Problem**: Text elements getting clipped at edges of round display
**Solution**: Move elements toward center and adjust container sizes
```xml
<!-- Working positioning for AM/PM on round display -->
<PartText x="115" y="88" width="78" height="50" alpha="255">
```

### 2. Color Implementation in WFF
**Working**: Direct color values in XML
```xml
<Font family="@font/imagine_font" size="45" color="#f5df1d">  <!-- Yellow -->
<Font family="@font/imagine_font" size="45" color="#ffffff">  <!-- White -->
```

### 3. Container Height for Text Elements
**Problem**: Text getting cut off vertically
**Solution**: Increase container heights beyond font size
```xml
<!-- For 45pt font, use 50px container height -->
<PartText x="0" y="0" width="450" height="50">
  <Font size="45">
```

### 4. Beam Animation Height Calculations
**Learning**: Beam heights must match time position changes
```xml
<!-- When time moves from y=114 to y=129 (+15px), increase beam height by 15px -->
<Group x="34" y="0" width="73" height="209" name="hour_tens_beam">
```

### 5. AM/PM Display Issues
**Problem**: Special characters appearing (', ‡) when trying to show/hide AM/PM
**Attempted Solutions**:
```xml
<!-- Failed: Using &apos; entities -->
<Template>%s<Parameter expression="[IS_24_HOUR_MODE] ? $&apos;&apos;$ : ([HOUR_0_23] < 12 ? $&apos;AM&apos;$ : $&apos;PM&apos;$)"/></Template>

<!-- Failed: Empty string with quotes -->
<Template><Parameter expression="[IS_24_HOUR_MODE] ? '' : ([HOUR_0_23] < 12 ? 'AM' : 'PM')"/></Template>

<!-- Partially working but shows apostrophe -->
<Template>%s<Parameter expression="[HOUR_0_23] < 12 ? 'AM' : 'PM'"/></Template>
```
**Current approach**: Using Variant to hide in 24-hour mode
```xml
<Variant expression="[IS_24_HOUR_MODE]" target="alpha" value="0"/>
```

### 6. Separating Symbols from Text
**Solution**: Use multiple PartText elements in same Group
```xml
<Group x="0" y="285" width="450" height="55" name="heart_rate_group">
  <!-- Counter text -->
  <PartText x="15" y="0" width="450" height="55">
    <Template>%d BPM<Parameter expression="[HEART_RATE_BPM]"/></Template>
  </PartText>
  <!-- Symbol with smaller font -->
  <PartText x="110" y="7" width="40" height="40">
    <Font size="35" color="#f5df1d">
      <Template>♥</Template>
    </Font>
  </PartText>
</Group>
```

### 7. Number Formatting in WFF
**Working formats**:
```xml
<!-- 3-digit battery with leading zeros -->
<Template>%03d%%<Parameter expression="[BATTERY_PCT]"/></Template>

<!-- 5-digit steps with leading zeros -->
<Template>%05d<Parameter expression="[STEP_COUNT]"/></Template>

<!-- 2-digit seconds -->
<Template>%02d<Parameter expression="[SECOND]"/></Template>
```

### 8. Sensor Data Permissions
**Added to AndroidManifest.xml**:
```xml
<uses-permission android:name="android.permission.BODY_SENSORS" />
<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
```
**Note**: Still showing 0 values - may need additional configuration or be emulator-related

### 9. Progress Bar Animation
**Working implementation**:
```xml
<Transform target="width" value="([SECOND] < 59) ? ([SECOND] * 7.5) : ([MILLISECOND] < 650) ? 450 : (450 * (1 - (([MILLISECOND] - 650) / 350.0)))" />
```

### 10. Font Size and Container Relationships
**Best practices discovered**:
- Container width should be ~5-10px wider than expected text
- Container height should be 5-10px taller than font size
- For symbols/emojis, may need even more padding

## Debugging Techniques Used

1. **Incremental positioning**: Moving elements in 5-10px increments
2. **Container visualization**: Temporarily increasing container sizes to see boundaries
3. **Build-test cycle**: Quick iterations with `./gradlew assembleDebug && adb install -r`
4. **Expression testing**: Trying different WFF expression syntaxes for AM/PM

## Tools and Commands

```bash
# Build and install cycle
./gradlew assembleDebug
export ANDROID_HOME=~/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/emulator:$ANDROID_HOME/platform-tools
adb install -r app/build/outputs/apk/debug/app-debug.apk

# Emulator management
emulator -list-avds
emulator -avd Wear_OS_Large_Round &
```

## Next Steps
See `remaining_issues_retro_beam.md` for the 6 outstanding issues to resolve.