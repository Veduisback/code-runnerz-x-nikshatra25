# Design Guidelines: Proximity Alert App for Blind Users

## Architecture Decisions

### Authentication
**No authentication required** - This is a single-user utility app with local-only functionality.

**Profile/Settings Screen Required:**
- Simple settings accessible via navigation
- Large touch targets (minimum 88pt tap area per Apple's accessibility guidelines)
- Settings include:
  - Alert sensitivity (distance threshold: Close, Medium, Far)
  - Alert volume control
  - Vibration intensity toggle
  - Voice speed for step counter announcements
  - Calibration option for step detection

### Navigation
**Stack-only navigation** - Linear, simple flow optimized for accessibility.

**Navigation Structure:**
1. **Camera Screen** (Main/Home) - Always active on launch
2. **Settings Screen** - Accessed via large settings button on camera screen
3. **Tutorial Screen** - Optional first-time walkthrough with audio instructions

**Rationale:** Minimal navigation reduces complexity for blind users. The camera screen should be the default view, always ready to use.

---

## Screen Specifications

### 1. Camera Screen (Main)
**Purpose:** Real-time proximity detection with audio/haptic alerts

**Layout:**
- **Header:** None - Full-screen camera view for maximum detection area
- **Main Content:** 
  - Full-screen camera preview (entire screen)
  - Non-scrollable fixed view
  - Floating elements overlay camera:
    - Large Settings button (top-right, 88pt × 88pt minimum)
    - Step counter display (top-left, large text for caregivers)
    - Visual proximity indicator (bottom-center, color-coded bar)

**Components:**
- Camera view (full viewport)
- Floating settings button (icon: gear/cog from @expo/vector-icons Feather set)
- Floating step counter text (updates in real-time)
- Proximity indicator bar (visual representation of distance - green/yellow/red)
- Active audio system (background, no UI)
- Haptic feedback system (background, no UI)

**Safe Area Insets:**
- Top: insets.top + Spacing.xl
- Bottom: insets.bottom + Spacing.xl
- Floating buttons respect safe areas

**Accessibility:**
- VoiceOver enabled for all interactive elements
- Settings button has audio label: "Open Settings"
- Screen reader announces proximity changes
- High-contrast visual elements for partially sighted users

---

### 2. Settings Screen
**Purpose:** Configure alert sensitivity, audio preferences, and app behavior

**Layout:**
- **Header:** Standard navigation header
  - Title: "Settings"
  - Left button: Back (< icon with "Camera" label)
  - Transparent background
- **Main Content:**
  - Scrollable form/list
  - Large, well-spaced controls
  - Each setting has audio description
  
**Components:**
- Section headers with dividers
- Slider controls (Alert Sensitivity, Volume)
- Toggle switches (Vibration, Voice Announcements)
- Picker for Voice Speed (Slow, Normal, Fast)
- Large "Calibrate Step Counter" button
- Large "Play Tutorial" button

**Safe Area Insets:**
- Top: headerHeight + Spacing.xl
- Bottom: insets.bottom + Spacing.xl

**Accessibility:**
- Minimum 24pt font size for all text
- High contrast labels
- Each control announces current value via VoiceOver
- Haptic feedback on toggle/slider changes

---

### 3. Tutorial Screen (Optional)
**Purpose:** Audio-guided walkthrough of app features

**Layout:**
- **Header:** Standard navigation header
  - Title: "How to Use"
  - Left button: Skip/Close
- **Main Content:**
  - Large, centered instructional text (for caregivers)
  - Audio playback controls (Play/Pause tutorial)
  - Progress indicator

**Safe Area Insets:**
- Top: headerHeight + Spacing.xl
- Bottom: insets.bottom + Spacing.xl

---

## Design System

### Color Palette
**Primary Function:** Visual feedback for caregivers and partially sighted users
- **Primary:** #007AFF (iOS blue) - Settings, interactive elements
- **Success/Safe:** #34C759 (green) - Object is far
- **Warning:** #FF9500 (orange) - Object is medium distance
- **Danger:** #FF3B30 (red) - Object is very close
- **Background:** #000000 (black) - Camera screen
- **Surface:** #1C1C1E (dark gray) - Settings screen background
- **Text Primary:** #FFFFFF (white) - High contrast
- **Text Secondary:** #8E8E93 (gray) - Less important info

### Typography
**Accessibility-first sizing:**
- **Display:** 48pt, bold - Step counter
- **Heading:** 32pt, semibold - Settings section headers
- **Body:** 24pt, regular - Settings labels, descriptions
- **Caption:** 18pt, regular - Secondary info

**Font:** System (SF Pro for iOS, Roboto for Android)

### Spacing
- **xs:** 4pt
- **sm:** 8pt
- **md:** 16pt
- **lg:** 24pt
- **xl:** 32pt
- **xxl:** 48pt

**Touch Targets:** Minimum 88pt × 88pt for all interactive elements

### Visual Design Principles
1. **High Contrast:** All UI elements use maximum contrast ratios (WCAG AAA compliant)
2. **Minimal UI:** Only essential visual elements; audio is primary interface
3. **Large Touch Targets:** All buttons are oversized for easy activation
4. **No Shadows:** Keep interface flat for maximum clarity
5. **Color-Coded Feedback:** Proximity indicator uses traffic light colors (green → yellow → red)

### Interaction Design
**Audio Feedback (PRIMARY INTERFACE):**
- **Proximity Alerts:**
  - Far (>3m): Low-pitched beep every 2 seconds
  - Medium (1-3m): Mid-pitched beep every 1 second
  - Close (<1m): High-pitched continuous ring
  - Alert volume increases as distance decreases
- **Step Counter:** Voice announces every 10 steps ("Ten steps," "Twenty steps," etc.)
- **UI Interactions:** Tap sound for button presses
- **System Announcements:** VoiceOver reads all screen changes

**Haptic Feedback (SECONDARY INTERFACE):**
- Light vibration: Object detected at medium distance
- Strong continuous vibration: Object very close (danger zone)
- Short pulse: Step counted
- Double pulse: Settings changed

**Visual Feedback (TERTIARY - for caregivers):**
- Proximity bar animates smoothly
- Color transitions from green → yellow → red
- Step counter increments with fade-in animation

### Critical Assets
**Audio Files (MUST GENERATE):**
1. `alert_far.mp3` - Low-pitched beep (300Hz, 0.2s)
2. `alert_medium.mp3` - Mid-pitched beep (600Hz, 0.3s)
3. `alert_close.mp3` - High-pitched ring (1200Hz, continuous)
4. `button_tap.mp3` - Subtle click sound
5. `step_count.mp3` - Example voice ("Ten steps") - Use TTS API for dynamic announcements

**Icons (from @expo/vector-icons Feather):**
- Settings: "settings"
- Back: "chevron-left"
- Play: "play-circle"
- Pause: "pause-circle"

**No custom images needed** - App is function-first, not decorative.

---

## Accessibility Requirements
1. **VoiceOver/TalkBack Support:** All screens fully navigable via screen reader
2. **Dynamic Type:** Text scales with system font size settings
3. **Reduce Motion:** Disable animations when system setting is enabled
4. **Audio Descriptions:** Every UI change has corresponding audio announcement
5. **Haptic Options:** Allow users to disable vibrations (for hearing aid users)
6. **Continuous Operation:** App prevents screen lock during active use
7. **Background Audio:** Alert sounds play even when app is in background