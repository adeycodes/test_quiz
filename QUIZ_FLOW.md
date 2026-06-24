# Year 8 Math Quiz - Enhanced Flow

## What's New?

### 1. Real-Time Answer Tracking ✓
Every time a student selects an answer option, it's immediately recorded:
- Radio button change → triggers `trackAnswers()`
- User answers stored in `userAnswers` object
- Maps: Question Number → Answer Index (0-3 for A-D)

### 2. Pre-Submission Confirmation Modal ✓
Before the student can submit, they see:

```
╔════════════════════════════════════════╗
║          Ready to Submit?              ║
║                                        ║
║   You have answered 35 out of 40      ║
║        questions.                      ║
║                                        ║
║  Do you want to submit your test,     ║
║  or go back and review?                ║
║                                        ║
║  [Review Answers]  [Submit Test]      ║
╚════════════════════════════════════════╝
```

**If they click "Review Answers":**
- Modal closes
- They return to the quiz
- They can make changes or add missing answers

**If they click "Submit Test":**
- Submission is finalized
- All answers are graded
- Results appear with detailed feedback

### 3. Answer Validation & Feedback ✓

After submission, for each question:

**Correct Answer:**
- Background: Green (emerald-50)
- Border: Green (emerald-300)
- Icon: Green checkmark ✓
- Score: +1 point

**Wrong Answer:**
- Your selection: Red background with X mark ✗
- Correct answer: Green background with label "← Correct"
- Score: 0 points

**Unanswered:**
- Correct answer: Amber background with label "← Missed"
- Score: 0 points

### 4. Teacher's Explanations ✓
After each question, a "Teacher's Note" appears with:
- Styled like handwritten notes on notebook paper
- Blue handwriting font (Kalam)
- Step-by-step solution walkthrough
- Helpful tips and reminders

## Example Question Flow

```
Question 1: "Solve 3x - 4 ≥ 11"
Options:
  A) x ≥ 5        ← Student selected this
  B) x ≤ 5
  C) x ≥ 7
  D) x > 5

After submission:
  A) x ≥ 5  ✓ [Green background, correct answer!]
  
Teacher's Note:
  "3x - 4 ≥ 11
   Add 4 to both sides:
   3x ≥ 15
   Divide by 3:
   x ≥ 5"
```

## Student Benefits

1. **Safety Net**: Can review before final submission
2. **Learning**: Sees correct answers immediately with explanations
3. **Transparency**: Knows exactly how many questions they answered
4. **Feedback**: Encouraging remarks based on performance
5. **Documentation**: Can print results or review later

## Technical Implementation

### New Variables
- `userAnswers` - Object storing all selected answers

### New Functions
- `trackAnswers()` - Captures current answers
- `submitExam()` - Shows confirmation modal
- `closeConfirmation()` - Returns to quiz
- `finalizeSubmit()` - Processes submission

### Modified Functions
- Updated radio buttons with `onchange="trackAnswers()"`
- Updated submit button to show confirmation modal first

## Code Locations

| Component | Location |
|-----------|----------|
| Confirmation Modal | Lines 178-195 |
| trackAnswers() | Lines 462-470 |
| submitExam() | Lines 472-483 |
| closeConfirmation() | Lines 485-487 |
| finalizeSubmit() | Lines 489+ |
| Radio onChange | Line 386 |
