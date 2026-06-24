# 10-Hour Session Management System

## Overview

Your Year 8 Mathematics quiz now has a robust session management system that keeps students logged in for **10 hours**. This prevents students from being redirected to the homepage while taking the quiz and reviewing their corrections.

## Features Implemented

### 1. **10-Hour Session Duration**
- Students are automatically given a 10-hour session when they enter their name and click "Let's Go!"
- The session is tracked in `localStorage` with an expiration timestamp
- Sessions automatically refresh every 60 seconds to extend the timeout
- Students can continue taking the quiz and reviewing answers without any interruptions

### 2. **Automatic Session Restoration**
- If a student's page accidentally reloads (browser crash, network issue, etc.), their session is automatically detected
- The quiz interface is restored to where they left off
- All previously answered questions are recovered
- The timer is restored to its last known state

### 3. **Quiz State Persistence**
- Every time a student answers a question, the answer is saved to `sessionStorage`
- The timer countdown is also saved along with answers
- If the page refreshes, all this data is automatically restored
- After submission, the quiz state is cleared but the session remains active for reviewing results

### 4. **Session Refresh Strategy**
- Sessions refresh every 60 seconds during the quiz (via timer update)
- Sessions refresh when students close a modal
- Sessions refresh when students view results (closeModal)
- Sessions refresh on page beforeunload to maintain continuity

## How It Works

### Session Creation
```javascript
// When student clicks "Let's Go!"
function startExam() {
    createSession(studentName);  // Creates 10-hour session
    // Quiz begins...
}
```

### Session Storage Structure
```javascript
{
    name: "Student Name",
    startTime: 1234567890000,
    expiresAt: 1234604490000,  // 10 hours later
    sessionId: "unique_id",
    hasStarted: true
}
```

### Quiz State Storage
```javascript
{
    sessionId: "unique_id",
    userAnswers: { 1: 0, 2: 1, 3: 2, ... },  // Question -> Answer mapping
    timeRemaining: 2400,  // Seconds left
    savedAt: 1234567890000
}
```

## Timeline of Student Experience

### Initial Session (Fresh Start)
```
1. Student loads quiz → Start screen shows
2. Student enters name and clicks "Let's Go!" → Session created (10 hours)
3. Quiz begins → Timer starts (1 hour countdown)
4. Every 60 seconds → Session refreshed automatically
```

### During Quiz
```
1. Student answers Question 1 → Saved to sessionStorage
2. Student answers Question 5 → Saved to sessionStorage
3. Page accidentally refreshes → Session and answers restored
4. Student continues from Question 6 → No data loss
5. Student clicks Submit → Confirmation modal appears
```

### After Submission
```
1. Student sees confirmation modal
2. Chooses "Submit Test" → Results calculated
3. Teacher notes appear with explanations
4. Student closes modal → Session still active
5. Student can leave page and return within 10 hours → Session still valid
6. After 10 hours → Session expires (student needs to start fresh)
```

## Security & Cleanup

### Session Expiration
- Sessions automatically expire after 10 hours
- An expired session is detected on page load and rejected
- Student must start fresh with a new session

### Quiz State Cleanup
- Quiz state (answers, timer) is cleared after final submission
- Session remains active for result review
- This prevents quiz state conflicts if student starts a new quiz

### Automatic Refresh
```javascript
// Every 60 seconds during quiz
if (timeRemaining % 60 === 0) {
    refreshSession();  // Extends the 10-hour timer
}
```

## Key Functions

| Function | Purpose |
|----------|---------|
| `createSession(name)` | Creates a new 10-hour session |
| `getSession()` | Retrieves and validates current session |
| `refreshSession()` | Extends session expiration by 10 hours |
| `clearSession()` | Removes session from localStorage |
| `checkSessionOnPageLoad()` | Restores session if valid on page load |
| `saveQuizState()` | Saves student answers and timer to sessionStorage |
| `restoreQuizState()` | Restores student answers and timer from sessionStorage |

## Console Logs (For Debugging)

The system logs session activities to the browser console:
```
[v0] Session created - expires in 10 hours at: 6/24/2026, 8:15:30 PM
[v0] Session refreshed - new expiry: 6/24/2026, 8:16:30 PM
[v0] Session restored - user: John Smith
[v0] Quiz state saved: {...}
[v0] Quiz state restored
[v0] Session cleared
```

## Testing Session Features

### Test 1: Session Creation
1. Open the quiz
2. Enter your name and click "Let's Go!"
3. Open browser DevTools → Application → LocalStorage
4. Look for `quizSession` entry with `expiresAt` timestamp 10 hours from now

### Test 2: Page Reload Recovery
1. Start the quiz and answer a few questions
2. Refresh the page (Ctrl+R or Cmd+R)
3. Quiz should restore to your previous state with all answers intact

### Test 3: Session Persistence
1. Start the quiz
2. Answer a question
3. Wait 2-3 minutes (or skip ahead to test)
4. Session should still be valid
5. Close and reopen browser → Session still active

## Technical Details

- **localStorage**: Stores session data (persistent across page reloads)
- **sessionStorage**: Stores quiz answers and timer (cleared only after submission)
- **10-hour duration**: 10 * 60 * 60 * 1000 milliseconds = 36,000,000 ms
- **Refresh interval**: Every 60 seconds (during timer countdown)
- **Session validation**: Checked on every page load

## Benefits for Students

✅ No accidental redirects to homepage  
✅ Automatic recovery from page crashes  
✅ Can take breaks within 10 hours and return  
✅ All answers saved during quiz  
✅ Session persists through corrections review  
✅ Transparent session management (no interference with user experience)

## Browser Compatibility

This feature uses standard browser APIs:
- `localStorage` - Supported in all modern browsers
- `sessionStorage` - Supported in all modern browsers
- `Date.now()` - Supported in all modern browsers

Works on:
- Chrome/Edge (version 4+)
- Firefox (version 3.5+)
- Safari (version 4+)
- Mobile browsers (iOS Safari, Chrome Mobile)

---

**Last Updated**: June 24, 2026  
**Session Duration**: 10 hours  
**Implementation**: localStorage + sessionStorage
