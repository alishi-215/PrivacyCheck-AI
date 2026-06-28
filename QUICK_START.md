# PrivacyCheck - Quick Start ⚡

## 5-Minute Setup

### 1. Get Free API Key (2 min)
- Go: https://console.groq.com/keys
- Sign up (free email signup)
- Copy your key (gsk_xxxxx)

### 2. Add Key to Code (1 min)
Open: `privacycheck/lib/groq_service.dart`

Line 11, change:
```dart
static const String apiKey = 'YOUR_GROQ_API_KEY_HERE';
```

To:
```dart
static const String apiKey = 'gsk_paste_your_key_here';
```

Save.

### 3. Build APK (2 min + wait time)
```bash
cd privacycheck
flutter pub get
flutter build apk --release
```

Wait 2-5 minutes. APK ready at:
```
build/app/outputs/flutter-apk/app-release.apk
```

### 4. Test It
- Transfer APK to phone or emulator
- Tap to install
- Open app
- Paste a scenario: "Can I use my friend's Netflix password?"
- Click Analyze
- Get ethical response ✓

---

## What the App Does

**Input:** Privacy/tech dilemma
**Output:** 
- Who is affected (stakeholders)
- Ethical frameworks to consider
- What can go wrong (risks)
- What you should actually do

---

## For Demo Day (5 June)

Just show:
1. Open PrivacyCheck
2. Paste: "Should I jailbreak my phone to bypass school monitoring?"
3. Click Analyze
4. Show structured response
5. Try another one: "A company wants my location data"

Takes 10 seconds per question. Class throws scenarios. App handles them.

---

## Submitting to Instructor

1. **APK file:** `build/app/outputs/flutter-apk/app-release.apk`
2. **Code folder:** Everything in `privacycheck/`
3. **Explanation:**
   - "Built ethics advisor using Flutter + Groq LLM"
   - "Prompt-engineered for consistent ethical analysis"
   - "Handles real student scenarios"

---

## If Anything Breaks

**App won't load:**
```bash
flutter clean
flutter pub get
flutter run
```

**API key error:**
- Check: https://console.groq.com/keys
- Copy full key (should be gsk_xxx)
- Paste into groq_service.dart line 11

**Too slow:**
- Normal. First call = 2-3 sec. Rest are faster.

---

## File Structure (Know This for Class)

```
privacycheck/
├── lib/
│   ├── main.dart              ← App UI + logic
│   └── groq_service.dart      ← LLM integration
├── android/                   ← Android config
├── pubspec.yaml               ← Dependencies
└── README.md                  ← Full docs
```

---

## What Makes This Good for Marks

✓ Real APK (not just code)
✓ LLM integrated (Groq API)
✓ Structured output (stakeholders | ethics | risks | action)
✓ Prompt engineering (tuned for ethics)
✓ Clean UI (material design)
✓ Error handling (network errors, invalid key)
✓ Works offline after response (only API call needed)
✓ Different domain (privacy, not generic workplace)

---

## Deadline: 5 June

Right now: **4 days left**

Timeline:
- Today: Get Groq key, add to code (**30 min**)
- Today/Tomorrow: Build APK (**5 min build + wait**)
- Tomorrow: Test on phone (**10 min**)
- Rest: Polish, demo practice, submit (**2 hours**)

You're ahead. Relax. 💯

---

Start now. Key → Code → Build → Done.

Questions? Check README.md
