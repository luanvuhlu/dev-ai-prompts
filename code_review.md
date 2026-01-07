## INPUT FORMAT

Provide your changes using ONE of these formats:

### Option 1: Git Diff (Preferred)
```diff
diff --git a/src/UserService.java b/src/UserService.java
index 1234567..abcdefg 100644
--- a/src/UserService.java
+++ b/src/UserService.java
@@ -10,7 +10,7 @@ public class UserService {
-    String query = "SELECT * FROM users WHERE id=" + userId;
+    PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id=?");
+    stmt.setInt(1, userId);
```

### Option 2: Before/After Code Blocks
**Before:**
```java
public void deleteUser(int id) {
    users.remove(id);
}
```

**After:**
```java
public void deleteUser(int id) {
    if (!users.containsKey(id)) {
        throw new UserNotFoundException(id);
    }
    users.remove(id);
}
```

### Option 3: Changed Files Description
- File: `payment/PaymentProcessor.java`
- Change: Added input validation for negative amounts
- Lines: 45-52

---

## CONTEXT (Required)

**PR Type:** [ ] Bug fix [ ] New feature [ ] Refactor [ ] Documentation [ ] Performance

**Deployment Risk:** [ ] Hotfix to production [ ] Regular release [ ] Experimental feature

**Code Maturity:** [ ] Prototype/MVP [ ] Production-critical [ ] Internal tool

---

## REVIEW OUTPUT STRUCTURE

### 1️⃣ Change Summary (2-3 sentences max)
Briefly describe what changed and why it matters.

Example: *"Added null checking to deleteUser() method. Previously would fail silently, now throws explicit exception."*

### 2️⃣ Issues Found (Priority-ordered)

Use this severity framework:

**🔴 BLOCKING (Must fix before merge):**
- Security vulnerabilities (SQL injection, XSS, auth bypass)
- Data corruption/loss risks
- Breaking API changes without migration
- Critical performance regressions (>50% slower)

**🟡 SHOULD FIX (Address before next release):**
- Logic bugs in non-critical paths
- Memory leaks
- Poor error handling
- Missing validation for important inputs

**🟢 OPTIONAL (Nice-to-have improvements):**
- Code style inconsistencies
- Minor performance optimizations (<10% gain)
- Better variable names
- Additional test coverage

**Format for each issue:**
```
🔴 **SQL Injection Vulnerability**
- **Location:** UserRepository.java:47
- **Problem:** String concatenation in SQL query allows injection
- **Current code:** `query = "SELECT * FROM users WHERE id=" + userId`
- **Fix:** Use PreparedStatement with parameter binding
- **Why it matters:** Attacker can extract entire database
- **Reference:** [OWASP SQL Injection Guide]
```

### 3️⃣ Code Quality Observations

**Positive aspects:**
- Well-structured, clear logic
- Good error handling
- Appropriate comments

**Suggestions (non-blocking):**
- Extract magic numbers to constants
- Consider adding unit tests for new edge cases
- Simplify nested conditionals

### 4️⃣ Decision & Rationale

**[ ] ✅ APPROVE** - No blocking issues, ready to merge

**[ ] 🔄 REQUEST CHANGES** - Fix 🔴 blocking issues first
- Priority 1: [Issue description]
- Priority 2: [Issue description]

**[ ] ❌ REJECT** - Fundamental design problems
- Reason: [Explain why this approach won't work]
- Alternative: [Suggest better approach]

### 5️⃣ Optional Enhancements (If applicable)

**Performance ideas:**
- Consider caching frequently accessed data
- Use bulk operations instead of loops

**Maintainability ideas:**
- Extract repeated logic to helper method
- Add debug logging for troubleshooting

**Testing ideas:**
- Add parameterized tests for boundary cases
- Consider integration test for database interaction

---

## REVIEW PRINCIPLES

**Focus on the diff only:**
- ✅ Comment on lines that changed
- ❌ Don't critique unchanged code unless directly related

**Be specific and actionable:**
- ✅ "Line 47: Use PreparedStatement to prevent SQL injection"
- ❌ "Security could be better"

**Provide context for suggestions:**
- ✅ "This N+1 query will slow down with >100 users. Consider JOIN or batch fetch."
- ❌ "Bad performance"

**Balance thoroughness with brevity:**
- Max 3-5 issues per severity level
- If more than 10 total issues, group similar ones
- Focus on high-impact changes

**Consider the PR context:**
- Hotfix: Only flag 🔴 blocking security/correctness issues
- Feature: Can suggest 🟡 improvements for maintainability
- Refactor: Focus on whether new code is actually clearer

---

## EXAMPLE REVIEW

**Input:**
```diff
- if (user.age > 18) {
+ if (user.age >= 18) {
      allowAccess();
  }
```

**Output:**

**1️⃣ Change Summary**
Fixed off-by-one error in age validation. Now correctly allows 18-year-olds access (legal adult age).

**2️⃣ Issues Found**

🟡 **Missing Null Check**
- **Location:** Line 23
- **Problem:** `user.age` could be null if user object incomplete
- **Fix:** Add `user != null && user.age != null` check before comparison
- **Why:** Prevents NullPointerException in production

**3️⃣ Code Quality**
✅ Correct fix for the boundary condition
💡 Consider extracting `LEGAL_AGE = 18` as a constant for easier updates if laws change

**4️⃣ Decision**
🔄 **REQUEST CHANGES** - Add null check (🟡 priority), then good to merge

**5️⃣ Enhancements**
- Add unit test: `testAccessAllowedAtExactly18Years()`
- Consider logging denied access attempts for analytics
```

---

Now paste your code diff or describe the changes:
```

## **Key Improvements in Revised Version**

### **1. Explicit Input Format (3 options)**
- Git diff (most common)
- Before/after blocks (clearest for small changes)  
- Description (when diff unavailable)

### **2. Context Questions**
- PR type: bug fix vs new feature
- Deployment risk: hotfix vs experimental
- Code maturity: MVP vs production-critical

These drastically change how strict the review should be.

### **3. Clear Severity Framework**
```
🔴 BLOCKING: Security, data loss, breaking changes
🟡 SHOULD FIX: Logic bugs, poor error handling
🟢 OPTIONAL: Style, minor optimizations
```

No more ambiguity about what's critical vs nice-to-have.

### **4. Structured Issue Format**
```
🔴 **SQL Injection Vulnerability**
- Location: UserRepository.java:47
- Problem: String concatenation allows injection
- Current code: query = "SELECT * FROM users WHERE id=" + userId
- Fix: Use PreparedStatement
- Why it matters: Attacker can extract entire database
```

This is copy-paste ready for developers to understand and fix.

### **5. Review Principles Section**
```
✅ Comment on lines that changed
❌ Don't critique unchanged code
```

Sets expectations for review scope and prevents tangential feedback.

### **6. Complete Example Review**
Shows the ENTIRE output format with a real diff, so users know exactly what to expect.

### **7. Brevity Guidelines**