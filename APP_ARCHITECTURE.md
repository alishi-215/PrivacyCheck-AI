# PrivacyCheck - Architecture & Design

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    USER (Flutter App)                        │
│  ┌──────────────────────────────────────────────────────┐   │
│  │ main.dart - UI Layer                                 │   │
│  │ • Text input for dilemma                             │   │
│  │ • Submit button triggers analysis                    │   │
│  │ • Loading spinner while waiting                      │   │
│  │ • Response display with Markdown formatting          │   │
│  └──────────────────────────────────────────────────────┘   │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTP POST
                         ▼
┌─────────────────────────────────────────────────────────────┐
│              groq_service.dart - API Layer                  │
│  • Handles Groq API calls                                   │
│  • Sends system prompt + user message                       │
│  • Error handling (invalid key, network)                    │
│  • Returns structured response                              │
└────────────────────────┬────────────────────────────────────┘
                         │ HTTPS Request
                         ▼
┌─────────────────────────────────────────────────────────────┐
│         Groq Cloud (Free Tier LLM Service)                  │
│  Model: Mixtral-8x7b-32768                                  │
│  • Fast inference (~1-2 sec)                                │
│  • 10,000 tokens/day free                                   │
│  • Accepts: System prompt + User message                    │
│  • Returns: Ethics analysis (structured)                    │
└─────────────────────────────────────────────────────────────┘
```

---

## Data Flow

### 1. User Input
```
User types: "Can I sell my location data to earn money?"
```

### 2. App Processing
```dart
// main.dart - User taps "Analyze"
_analyzeDislemma() {
  // Get text from input field
  final userInput = _controller.text;
  
  // Call API service
  final result = await GroqService.analyzePrivacyDilemma(userInput);
  
  // Update UI with response
  setState(() { _response = result; });
}
```

### 3. API Call
```dart
// groq_service.dart
final response = await http.post(
  'https://api.groq.com/openai/v1/chat/completions',
  headers: {
    'Authorization': 'Bearer $apiKey',
    'Content-Type': 'application/json',
  },
  body: {
    'model': 'mixtral-8x7b-32768',
    'messages': [
      {
        'role': 'system',
        'content': 'You are PrivacyCheck, ethics advisor...'
      },
      {
        'role': 'user',
        'content': 'Can I sell my location data to earn money?'
      }
    ]
  }
);
```

### 4. LLM Response
```
**STAKEHOLDERS**
- You (person selling data)
- Companies buying data
- Other users (privacy collective)

**ETHICAL FRAMEWORKS**
- Deontological: Personal ownership of data
- Consequentialist: Risk vs reward
- Virtue Ethics: Integrity in privacy

**PRIVACY & SECURITY RISKS**
- Data resale to 3rd parties
- Identity theft risk
- Tracking by advertisers
- Manipulation via targeted ads

**WHAT YOU SHOULD DO**
Don't sell personal location data. The money won't cover the privacy loss. Instead, use privacy-respecting apps and opt out of data collection.
```

### 5. UI Display
App shows formatted response with:
- Bold headers
- Bullet points
- Clear action items

---

## Prompt Engineering

### System Prompt Design
```
You are PrivacyCheck, an AI ethics advisor specializing in 
privacy, data, and tech ethics for students.

[Output format specification]
[Guidelines: concise, practical, not preachy]
```

**Why this works:**
- Defines role → consistent behavior
- Specifies output format → structured responses
- Limits scope → only privacy/ethics
- Sets tone → practical advice

### Temperature Setting
```
temperature: 0.7
```
- 0.0 = deterministic (same answer every time)
- 0.7 = balanced (creative but consistent)
- 1.0 = random (unpredictable)

We use 0.7 because:
- Ethics questions need thought, not randomness
- But can vary approach for different scenarios
- Avoids robotic/repetitive responses

### Token Limit
```
max_tokens: 1024
```
- Limits response size
- Keeps analysis concise
- Prevents token waste
- Saves cost

---

## Error Handling

### Invalid API Key
```dart
if (response.statusCode == 401) {
  return 'Error: Invalid or expired API key.';
}
```

### Network Error
```dart
catch (e) {
  return 'Network Error: $e';
}
```

### Empty Input
```dart
if (userInput.trim().isEmpty) {
  setState(() { _error = 'Please describe your dilemma'; });
}
```

---

## State Management

```dart
class _PrivacyCheckHomeState extends State<PrivacyCheckHome> {
  String _response = '';        // Stores LLM response
  bool _isLoading = false;      // Shows loading spinner
  String _error = '';           // Stores error messages
  
  // setState() rebuilds UI when state changes
}
```

**Flow:**
1. User types → no state change (just text field)
2. User clicks Analyze → `_isLoading = true` (show spinner)
3. API responds → `_isLoading = false`, `_response = ...`
4. UI rebuilds → shows response

---

## UI Components

### Material Design 3
- Used in pubspec.yaml: `useMaterial3: true`
- Modern, clean look
- Dark mode support automatic
- Responsive design

### Key Widgets
```
Scaffold (main structure)
├─ AppBar (header with title)
├─ SingleChildScrollView (scrollable content)
│  └─ Column (vertical layout)
│     ├─ Card (input section header)
│     ├─ TextField (user input)
│     ├─ Wrap (quick example chips)
│     ├─ Row (buttons)
│     └─ Card (response area)
```

### Response Formatting
```dart
MarkdownBody(
  data: _response,  // Auto-renders: **bold**, - bullets, etc.
)
```

---

## Performance Metrics

| Metric | Value | Note |
|--------|-------|------|
| API Response Time | 1-3 sec | First call slower (cold start) |
| Token Usage per Query | ~200-400 | 10,000/day free tier = ~25+ queries |
| App Size | ~150-200 MB | Typical Flutter release build |
| Memory Usage | ~100-150 MB | Runtime, varies by OS |
| Startup Time | <2 sec | App launches fast |

---

## Security Considerations

### API Key Management
```
❌ DON'T: Commit API key to public GitHub
✓ DO: Use environment variables in production
```

For class: Can include in code (educational project)

### API Encryption
```
✓ HTTPS only (api.groq.com)
✓ Headers include auth token
✓ No hardcoded sensitive data in responses
```

### User Data
```
✓ No data stored locally
✓ No analytics sent
✓ No user tracking
✓ Queries sent to Groq (their privacy policy applies)
```

---

## Testing Scenarios

### Test 1: Valid Dilemma
Input: "Should I jailbreak my phone?"
Expected: Structured analysis with 4 sections

### Test 2: Invalid Key
Expected: Error message directing to console.groq.com

### Test 3: No Internet
Expected: Network error message

### Test 4: Empty Input
Expected: Validation error

### Test 5: Long Dilemma
Input: Multi-paragraph scenario
Expected: Still works (api handles up to 2000 tokens)

---

## Deployment Checklist

- [x] API key placeholder in code ← User adds real key
- [x] Error handling for common failures
- [x] UI responsive (works on phone + tablet)
- [x] Dark mode support
- [x] Prompt tested for consistent output
- [x] APK buildable without additional config
- [x] README with full setup instructions
- [x] No hardcoded secrets in repo

---

## For Demo Day

**Show:**
1. App opens → PrivacyCheck title + description
2. Paste scenario → "Can I use my friend's streaming account?"
3. Click Analyze → Loading spinner appears
4. 2 seconds later → Structured response appears
5. Quick Examples chips → Click to auto-fill
6. Try different scenario → Same process
7. Show error handling → No internet error message

**Talk about:**
- Flutter (cross-platform)
- Groq API (free LLM)
- Prompt engineering (system prompt)
- Structured output (4-section format)
- Real-world use (student ethics advisor)

---

## Architecture Summary

**Clean Separation:**
- UI Layer (main.dart) ← What user sees
- Service Layer (groq_service.dart) ← How it talks to API
- External Service (Groq) ← Brain of the operation

**Scalable:**
- Can add more analysis types
- Can switch LLM provider (same service layer)
- Can add local database for history

**Tested:**
- Error paths covered
- Network failures handled
- Invalid input rejected
- Response formatted correctly

---

**That's the whole system. Simple, clean, effective. 🎯**
