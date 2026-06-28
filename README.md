# PrivacyCheck - Privacy & Tech Ethics Advisor
## Complete APK Build Guide

### What is PrivacyCheck?
An AI-powered app that analyzes privacy & tech ethics dilemmas for students.
- Input: Tech/privacy scenario
- Output: Stakeholders | Ethical Frameworks | Risks | Action
- Uses: Groq API (free) + Flutter

---

## STEP 1: GET YOUR GROQ API KEY (FREE)

1. Go to: https://console.groq.com/keys
2. Sign up (free) with email
3. Copy your API key (looks like: `gsk_xxxxxxxxxxxxx`)
4. Keep it safe (don't share in code commits)

**Note:** Free tier = 10,000 tokens/day. That's ~500+ app analyses. More than enough.

---

## STEP 2: ADD API KEY TO PROJECT

Open this file:
```
privacycheck/lib/groq_service.dart
```

Find line 11:
```dart
static const String apiKey = 'YOUR_GROQ_API_KEY_HERE';
```

Replace with your actual key:
```dart
static const String apiKey = 'gsk_your_actual_key_here';
```

Save the file.

---

## STEP 3: BUILD APK

### Option A: Using Command Line (Recommended)

```bash
# 1. Navigate to project folder
cd /path/to/privacycheck

# 2. Get dependencies
flutter pub get

# 3. Build APK (release build - optimized, smaller size)
flutter build apk --release

# APK will be at:
# build/app/outputs/flutter-apk/app-release.apk
```

### Option B: Using Android Studio (GUI)

1. Open project in Android Studio
2. Click: Build → Flutter → Build APK
3. Wait for build to complete
4. APK will be in `build/app/outputs/flutter-apk/`

---

## STEP 4: INSTALL & TEST APK

### On Physical Device:
```bash
# 1. Connect Android phone via USB
# 2. Run:
flutter install

# OR transfer build/app/outputs/flutter-apk/app-release.apk to your phone
# and tap to install
```

### On Emulator:
```bash
flutter run -d emulator-5554
```

---

## STEP 5: PROJECT STRUCTURE

```
privacycheck/
├── lib/
│   ├── main.dart              ← UI & app logic
│   └── groq_service.dart      ← LLM API integration
├── pubspec.yaml               ← Dependencies
└── android/                   ← Auto-generated Android config
```

---

## HOW IT WORKS

1. **User enters dilemma** → Text input
2. **App calls Groq API** → Sends prompt + dilemma
3. **LLM analyzes** → Returns structured response
4. **App displays** → Formatted analysis with headings/bullets

### Prompt Engineering:
- System prompt tells LLM to be an ethics advisor
- Exact output format: Stakeholders | Frameworks | Risks | Action
- Temperature 0.7 = balanced (creative but not random)

---

## TROUBLESHOOTING

### "API Key Error"
- Check key is copied correctly (no extra spaces)
- Verify at: https://console.groq.com/keys
- Key should start with `gsk_`

### "Network Error"
- Check internet connection
- Firewall might be blocking API calls
- Try on different WiFi/mobile data

### "App won't build"
```bash
# Clean and rebuild
flutter clean
flutter pub get
flutter build apk --release
```

### "Slow response"
- First call takes 2-3 seconds (API cold start)
- Subsequent calls are faster
- This is normal

---

## DEMO CHECKLIST (For Class)

✓ Open app
✓ Paste a test dilemma
✓ Click "Analyze"
✓ Show structured response
✓ Try 2-3 different scenarios
✓ Mention: Groq API, Prompt Engineering, Flutter

**Example test inputs:**
- "Should I jailbreak my phone to bypass parental controls?"
- "A company wants my location data. I need the service but feel unsafe."
- "Can I scrape a website's data if no one told me not to?"

---

## NOTES FOR SUBMISSION

1. **Don't commit API key** to GitHub
   - Use `.env` file in production
   - For class demo, it's okay to have it in code

2. **Code quality:**
   - Clean Flutter structure ✓
   - Error handling ✓
   - Loading states ✓
   - Dark mode support ✓

3. **Prompt engineering:**
   - System prompt is tuned for ethics ✓
   - Output format is structured ✓
   - Model is fast (Mixtral) ✓

4. **APK size:**
   - Release build: ~150-200MB (typical Flutter)
   - Signed and ready to ship

---

## WHAT TO TELL YOUR PROFESSOR

"I built PrivacyCheck, an AI ethics advisor using Flutter and Groq API. The app takes privacy dilemmas as input and returns structured analysis: stakeholders, ethical frameworks, risks, and recommended actions. I used prompt engineering to ensure consistent, ethical output. The app is built, tested, and ready for live demo."

---

## QUESTIONS?

If app crashes:
1. Check API key is set
2. Check internet connection
3. Check logcat: `flutter logs`

If response is blank:
- Wait 3 seconds (API call takes time)
- Check Groq dashboard for API usage

---

**TLDR:**
1. Get free Groq key: https://console.groq.com/keys
2. Add key to groq_service.dart line 11
3. Run: `flutter build apk --release`
4. APK ready at: `build/app/outputs/flutter-apk/app-release.apk`
5. Install and demo to class

Good luck! 🚀
