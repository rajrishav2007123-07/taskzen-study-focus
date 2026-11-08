# TaskZen Study

**Version 2.0.0** · targetSdk 36 · minSdk 26 · Java 17

A focused study productivity app for JEE / NEET / Engineering / Business cohorts.  

---

## Features

- **Task timers** with break tracking, single-running-task rule, expected-time warnings
- **Cohorts**: JEE, NEET, B.Tech/Engineering, Startup/Business/Corporate
- **Home-screen widgets**: daily task progress + circular exam countdown
- **Alarms & reminders**: exact alarms, continuous vibration (foreground service), personal alarms, math-challenge stop
- **Library / continuous mode** for long study sessions
- **Export / import** JSON + auto safety backup to Downloads
- **AdMob** banners, interstitial, rewarded, native (no app-open ads)
- **Themes, reports, hourly motivation**

All workflows above are preserved from the production 1.6.x line.

---

## Project structure

```
app/
  src/main/
    assets/index.html          # Full study UI + business logic
    java/com/jeetracker/app/   # Native bridge, widgets, alarms, ads
    res/                       # Layouts, drawables, widgets, themes
  build.gradle
  proguard-rules.pro
```

Native responsibilities:
| Class | Role |
|-------|------|
| `MainActivity` | WebView host, permissions, file chooser |
| `AndroidBridge` | `@JavascriptInterface` surface for the Web UI |
| `AdManager` | AdMob banners / interstitial / rewarded / native |
| `VibrationService` | Foreground continuous vibration (Doze-safe) |
| `*WidgetProvider` | Home-screen widgets |
| `*Alarm*` / `*Receiver` | Exact alarms, boot restore, notifications |
| `NotificationHelper` | Channels and notification actions |

---

## Build

```bash
cp local.properties.example local.properties
# edit sdk.dir=...

./gradlew clean bundleRelease
# AAB → app/build/outputs/bundle/release/app-release.aab
```

Debug:
```bash
./gradlew assembleDebug
```

---

## Google Play (API 36 / 2026) checklist

| Requirement | Status in this project |
|-------------|------------------------|
| targetSdk / compileSdk 36 | ✅ |
| 16 KB page size | ✅ (pure Java + WebView) |
| SCHEDULE_EXACT_ALARM only (no USE_EXACT_ALARM) | ✅ |
| WebView debugging only in debug builds | ✅ |
| Cleartext traffic disabled | ✅ |
| R8 minify + shrink for release | ✅ |
| Foreground service `specialUse` declared | ✅ (still declare in Play Console) |
| AAB packaging | Use `bundleRelease` |

**You must still complete in Play Console:**
2. Privacy policy URL
3. Account deletion path (if using Auth)
4. Special-use FGS declaration
5. Ads / Advertising ID declaration

Replace `privacy_policy_url` in `res/values/strings.xml` with your real policy.

---

## Configuration

- **AdMob App ID** – `AndroidManifest.xml` meta-data  
- **Ad unit IDs** – `AdManager.java` constants  

Never embed service-account keys or admin passwords in the APK.

---

## License / notes

Production-ready structure for Play Store submission.  
Business logic lives in `index.html` by design so the proven study workflows remain stable.


## v2.0.4 Language Selector
Added a top-right language selector with 10 languages (English first): English, Hindi, Bengali, Telugu, Marathi, Tamil, Gujarati, Kannada, Malayalam and Punjabi. Selection is persisted locally.
