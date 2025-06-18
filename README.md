# Retro Beam

A feature-rich Wear OS watch face that combines classic sci-fi beam animations with modern health and fitness tracking in a beautiful retro aesthetic.

## Description

Retro Beam is an evolved version inspired by the original [Beam Up watch face](https://github.com/C-D-Lewis/beam-up) created by [Chris Lewis](https://github.com/C-D-Lewis) for the [Pebble smartwatch](https://getpebble.com/). This implementation brings the iconic tractor beam animation effect to modern Wear OS devices while adding comprehensive health tracking and a refined retro design.

The watch face displays time in a bold digital format using the classic Imagine font. When digits change, yellow tractor beams animate upward from the bottom of the screen, creating the signature sci-fi "beam up" effect. Beyond the original concept, Retro Beam integrates modern smartwatch features with a carefully crafted yellow and white color scheme.

### Key Features

- **Digital Time Display**: Large, easy-to-read yellow digits with beam animations
- **Smart Health Tracking**: Real-time display of heart rate, step count, and battery level
- **AM/PM Indicator**: Convenient 12/24-hour format support with visual indicator
- **Live Seconds Counter**: Real-time seconds display and animated progress bar
- **Day & Date Display**: Current day of week and date information
- **Retro Color Scheme**: Beautiful yellow (#f5df1d) and white theme
- **Beam Animation**: Four-phase animation sequence when digits change:
  - Phase 1: Beam extends upward to reach the old digit
  - Phase 2: Old digit fades out as it's "transported"
  - Phase 3: New digit fades in as it's "materialized"
  - Phase 4: Beam retracts back down
- **Optimized Layout**: Pixel-perfect positioning for round Wear OS displays
- **Hardware Acceleration**: Smooth 60fps animations and rendering

### Health & Fitness Integration

- **Heart Rate Monitoring**: Displays current heart rate with ♥ symbol
- **Step Counter**: Shows daily step count with 👟 symbol
- **Battery Indicator**: Real-time battery percentage display
- **Sensor Permissions**: Full integration with Wear OS health sensors

## Animated Preview

![Retro Beam Animation](screenshots/animation.gif)

## Building and Installation

### Prerequisites

1. **Android Studio** - Download and install [Android Studio](https://developer.android.com/studio)
2. **Android SDK** - Installed automatically with Android Studio, or install the [command line tools](https://developer.android.com/studio#command-tools)
3. **Java Development Kit (JDK)** - JDK 17 or higher (included with recent Android Studio versions)

### Build Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/jeremymanning/retro-beam.git
   cd retro-beam
   ```

2. Navigate to the watch face project directory:
   ```bash
   cd RetroBeam
   ```

3. Build the APK using Gradle:
   ```bash
   ./gradlew assembleDebug
   ```

   On Windows, use:
   ```bash
   gradlew.bat assembleDebug
   ```

4. The APK will be generated at:
   ```
   app/build/outputs/apk/debug/app-debug.apk
   ```

### Installation

#### Option 1: Install via ADB (Recommended)

1. Enable Developer Options on your Wear OS device:
   - Go to Settings > System > About
   - Tap the Build number 7 times
   - Go back to Settings > Developer options
   - Enable ADB debugging

2. Connect your watch via USB or [WiFi debugging](https://developer.android.com/training/wearables/get-started/debugging)

3. Install the watch face:
   ```bash
   ./gradlew installDebug
   ```

#### Option 2: Manual APK Installation

1. Transfer the APK file to your watch
2. Use a file manager app on the watch to install the APK
3. Grant necessary permissions when prompted (body sensors, activity recognition)

### Selecting the Watch Face

1. Long press on your current watch face
2. Swipe to browse available watch faces
3. Select "Retro Beam" from the list
4. Grant health sensor permissions if prompted
5. Enjoy your new retro sci-fi watch face!

## Technical Details

- **Platform**: Wear OS 3.0+ (API 30+)
- **Implementation**: Built using the [Watch Face Format (WFF)](https://developer.android.com/training/wearables/wff) - Android's declarative XML format for watch faces
- **Rendering**: Hardware-accelerated canvas with optimized animations
- **Sensor Integration**: Real-time health data via Wear OS sensor APIs
- **Font**: Imagine font (included) for authentic retro aesthetics
- **Permissions**: `BODY_SENSORS`, `ACTIVITY_RECOGNITION` for health data

### Sensor Data Sources

- **Battery**: `[BATTERY_PERCENT]` - Current battery level (0-100%)
- **Heart Rate**: `[HEART_RATE]` - Current heart rate in BPM
- **Step Count**: `[STEP_COUNT]` - Daily step counter
- **Time Data**: Live seconds, AM/PM indicators, day/date information

## Credits

- **Original Concept**: [Chris Lewis](https://github.com/C-D-Lewis) - Creator of the original [Beam Up watch face](https://github.com/C-D-Lewis/beam-up) for Pebble
- **Retro Beam Implementation**: Enhanced with modern health tracking and refined design
- **Font**: Imagine font by Nate Halley
- **Development**: Built with extensive testing and optimization for Wear OS devices

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.