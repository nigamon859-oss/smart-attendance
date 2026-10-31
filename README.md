# Smart Attendance System (v2.4.0)
### Swiss International Typographic Edition // Offline-First Academic Tracker

Smart Attendance is a high-precision, low-resource student timetable and attendance tracking system. Designed following the principles of **Swiss International Typographic Modernism**, it provides real-time algorithmic attendance telemetry, deficit prevention alerts, timetable schedule monitoring, optical timetable scanning (OCR), and calendar synchronization—**running 100% locally on-device with zero internet dependency**.

---

## Key Features

- **Daily Routine Ledger (Tab 01):**
  - Live indication of current and upcoming lectures.
  - Three-state logging: **YES** (Attended), **NO** (Bunked/Missed), **EXEMPT** (Professor Absent / Class Cancelled).
  - Unmark toggle: Tap the same status again to unmark an accidental selection without penalty.
  - Routine Day Switcher: Inspect and simulate routine logging for any day (Monday through Saturday).
  - **Full-Day Celebration (Requirement 10):** Swipable modal with randomized appreciation quotes when 100% daily attendance is completed.

- **Timetable Matrix & Course Registry (Tab 02):**
  - Full weekly matrix (Monday to Saturday) with clean period intervals, lecture halls, and instructor details.
  - Interval overlap detection: Automatically prevents conflicting slot timings.
  - Dynamic subject editor with real-time stats calculation.

- **Calendar & Device Sync (Tab 03):**
  - Interactive monthly calendar grid displaying academic recesses, holidays, and past daily attendance marks.
  - **RFC 5545 `.ics` Export:** 1-click export of your weekly timetable into Google Calendar, Apple Calendar, or Outlook with automated 10-minute pre-class alarms.

- **Optical Timetable Scanner / OCR Engine (Tab 04):**
  - Upload or snap a picture of your printed university timetable.
  - Off-screen canvas preprocessing with contrast binarization.
  - Neural OCR extraction (Tesseract.js) with fallback to an intelligent Heuristic Structural Matrix Parser.
  - Interactive candidate slots review with 1-click commit into your active routine.

- **Student Profile & Academic Configuration (Tab 05):**
  - Student identity headers: Name, USN / Roll Number, Department, Semester.
  - Minimum criteria compliance cutoff slider (50% – 95%).
  - Notification parameters: 5/10/15/20 minute pre-class lead times, 5-day vs 6-day academic weeks.
  - **New Semester Reset:** Resets attendance numbers to 0 for a new semester while preserving course subjects and timetable routines.
  - **Reset Today's Logs:** Resets markings for the current date without modifying cumulative stats.
  - **JSON Telemetry Backup:** Complete offline state export and restore.

- **Notification Harness & Deficit Warnings (Requirements 11 & 13):**
  - Pre-class lead time alerts: *"You have a class scheduled: [Subject] at [Time]"*.
  - Post-class attendance prompt: *"2L [Subject] attendance updated. Did you attend this unit?"*.
  - Urgent Deficit Alert: When subject attendance falls below the cutoff, triggers:
    *"Attend this class if you don't want trouble! Attendance is at X% (Below Y% cutoff)."*

- **Bug & Diagnostic Reporting (Requirements 6 & 9):**
  - Integrated bug report modal sending diagnostic telemetry directly to developer email via Formspree (`https://formspree.io/f/xvgopkzw`).
  - Offline fallback queue: If offline, report is queued for dispatch when network is available.

---

## How to Share with Friends & Install on Android

### Method 01: Host Free on GitHub Pages (Easiest for Friends — 1-Minute Install)

1. **Push this project to a GitHub repository**:
   ```bash
   git init
   git add .
   git commit -m "feat: initial commit of smart attendance v2.4"
   git branch -M main
   git remote add origin https://github.com/<your-username>/smart-attendance.git
   git push -u origin main
   ```
2. In your GitHub repository:
   - Go to **Settings** > **Pages**.
   - Under **Build and deployment** > **Branch**, select `main` and root `/`.
   - Click **Save**.
3. **Send the GitHub Pages link to your friend**:
   `https://<your-username>.github.io/smart-attendance/`
4. **On their Android phone**:
   - Open the link in Google Chrome.
   - An install banner will pop up automatically: tap **"Install App"** (or tap the 3 dots **⋮** > **"Add to Home screen"** / **"Install app"**).
   - Android will automatically compile and install a real standalone **WebAPK** onto their home screen with the custom frosted glass monogram icon.
   - It works **100% offline**, uses minimal RAM, and requires no Google Play Store!

---

### Method 02: Generate Downloadable Standalone `.apk` File (Zero Tools Needed)

If you or your friend want a standalone `.apk` file to download and share over WhatsApp or Drive:

1. Host the project or deploy to GitHub Pages (as shown in Method 01).
2. Go to **[PWABuilder.com](https://www.pwabuilder.com)**.
3. Paste your GitHub Pages URL (e.g. `https://<your-username>.github.io/smart-attendance/`).
4. Click **"Package for Android"**.
5. PWABuilder generates a signed, installable `.apk` file that can be downloaded and transferred to any Android phone.

---

### Method 03: Run Locally on Wi-Fi (Install directly from your computer)

If you and your friend are on the same Wi-Fi network:

1. Run the local launcher:
   ```bash
   ./launch_app.sh
   ```
2. The terminal will display your computer's local network IP address:
   ```text
   >>>  http://192.168.x.x:8080  <<<
   ```
3. Have your friend open that address in Chrome on their Android phone.
4. Tap **"Install App"** or **"Add to Home screen"** to install the standalone app immediately!

---

### Method 04: Build Native Android APK via Flutter

If you or your friend use Android Studio or Flutter CLI:

```bash
cd flutter_app
flutter build apk --release
```
The output APK is generated at:
`flutter_app/build/app/outputs/flutter-apk/app-release.apk`

---

## Project Structure

```text
smart-attendance/
├── index.html               # Main UI markup (Swiss International Typographic grid, 5 panes)
├── styles.css               # Modernist stylesheet (Zero glassy effects, ultra-low RAM footprint)
├── app.js                   # Application state engine, OCR parser, sound synthesizer, telemetry
├── manifest.json            # PWA manifest for standalone Android installation
├── service-worker.js        # Offline asset caching & notification action handling
├── icon.svg                 # Frosted glass vector icon with geometric SA monogram
├── launch_app.sh            # One-line server launcher for LAN testing & WebAPK install
├── APK_GENERATION_GUIDE.md  # Detailed standalone APK compilation guide
├── README.md                # Project documentation & GitHub quick-start
└── flutter_app/             # Flutter implementation
    ├── pubspec.yaml
    └── lib/
        ├── main.dart
        ├── models/          # Subject & TimetableSlot data models
        ├── screens/         # Dashboard, Timetable, Holidays, Scanner, Config screens
        ├── theme/           # Swiss monochrome color system
        └── widgets/         # Segmented telemetry cards & custom widgets
```

---

## Privacy & Local Data Architecture

- **100% Offline Client Sandbox:** All timetable data, daily logs, holiday calendars, and student settings are stored directly in the browser's `localStorage` on the student's physical device.
- **Zero Telemetry Leaks:** No personal identity data or attendance logs ever leave the student's device.
- **Fail-Safe Storage:** Includes safe in-memory fallback wrappers to ensure zero crashes even when running in sandboxed or third-party webview contexts.
- **Bug Reporting:** Only when the user explicitly clicks "Send Bug Report" in the diagnostic sheet is a report dispatched to the linked Formspree endpoint.

---

## Developers & Attribution

- **Developer:** Sohan Gowda & Google DeepMind Antigravity Pair
- **Version:** 2.4.0 (Production Release)
- **Design Language:** Swiss International Typographic Modernism (Helvetica / Inter / Monospace)
- **Target OS:** Android (Standalone WebAPK / Native APK / PWA) & Modern Browsers
