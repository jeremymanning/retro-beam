# DISCOVERY: PartText Alpha Animation Pattern

## Working Pattern Found in Complications Sample

**File**: `/wear-os-samples/WatchFaceFormat/Complications/watchface/src/main/res/raw/watchface.xml`

### Working PartText Alpha Structure
```xml
<PartText x="0" y="33" width="112" height="20" alpha="0">
  <Variant mode="AMBIENT" target="alpha" value="255" />
  <Text align="CENTER" ellipsis="TRUE">
    <Font family="SYNC_TO_DEVICE" size="20" weight="NORMAL" slant="NORMAL" color="#ffffff">
      <Template>%s<Parameter expression="[COMPLICATION.TITLE]"/></Template>
    </Font>
  </Text>
</PartText>
```

### Key Insights
1. **PartText supports alpha attribute** (0-255)
2. **Variant can target alpha on PartText elements**
3. **Multiple PartText elements** can be used with different alpha values for visibility effects

### Pattern for Digit Animation
Based on this discovery, we could potentially create digit animation using:

```xml
<!-- Old digit - visible normally, hidden during beam phases 2-3 -->
<PartText x="0" y="0" width="90" height="100" alpha="255">
  <Variant target="alpha" value="0" condition="([SECOND] == 59) && ([MILLISECOND] >= 650) && ([MILLISECOND] < 1000)"/>
  <Text align="CENTER">
    <Font family="@font/imagine_font" size="117" color="#ffffff">
      <Template>[MINUTE_1]</Template>
    </Font>
  </Text>
</PartText>

<!-- New digit - hidden normally, visible during phase 4 -->
<PartText x="0" y="0" width="90" height="100" alpha="0">
  <Variant target="alpha" value="255" condition="([SECOND] == 0) && ([MILLISECOND] < 175)"/>
  <Text align="CENTER">
    <Font family="@font/imagine_font" size="117" color="#ffffff">
      <Template>[MINUTE_1]</Template>
    </Font>
  </Text>
</PartText>
```

### Critical Question
**Do Variant conditions support time-based expressions like `[SECOND]` and `[MILLISECOND]`?**

All examples found use `mode="AMBIENT"` for condition evaluation. We need to test if Variant supports:
- Time-based expressions (`[SECOND] == 59`)
- Complex conditions with `&&` operators
- Millisecond-precise timing

### Next Session Plan
1. **Test Variant with time expressions**: Start with simple `[SECOND]` condition
2. **Test PartText alpha animation**: Use the discovered pattern
3. **Test position animation**: Combine with Transform on Group if needed
4. **Progressive complexity**: Build from simple to complex timing

### Alternative Approaches if Variant doesn't support time expressions
1. **Research Transform target="alpha"** on PartText elements
2. **Look for other animation elements** in WFF specification  
3. **Multiple static text elements** with conditional visibility using other mechanisms

## Major Breakthrough
This discovery provides the exact XML structure pattern we need for text visibility animation. The key is using **PartText with alpha + Variant targeting alpha** - the same approach our working beams use!