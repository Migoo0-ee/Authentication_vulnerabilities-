# Authentication Vulnerabilities

> Study notes — Kayan Security Track
> Dates covered: 2026/10/17 → 2026/10/28

---

## 1. What is Authentication?

**Authentication** is the process of verifying the identity of a person.

### Main Types of Authentication Factors

| Factor | Description | Examples |
|---|---|---|
| **Knowledge Factor** | Something you *know* | Password, PIN, security question |
| **Possession Factor** | Something you *have* | Phone, security token |
| **Inherence Factor** | Something you *are* or *do* | Biometrics, behavioral patterns |

Authentication relies on a range of technologies to verify one or more of these factors.

---

## 2. Authentication vs Authorization

- **Authentication →** *Who* are you? (verifies identity → `user = valid`)
- **Authorization →** *What* are you allowed to do? (determines permissions → `admin / normal user`)

### How Authentication Vulnerabilities Arise

1. **Authentication mechanism is weak / fails** to protect against Brute-Force attacks.
2. **Poor code implementation or logic flaws** that allow an attacker to bypass authentication entirely → **Broken Authentication**.

---

## 3. Impact of Authentication Vulnerabilities (AV)

- **Subject of auth vulnerabilities:**
  - → Full account takeover: data, functionality, and full application control (Admin access)
  - → Access to sensitive data
  - → Expand the attack surface

- **Login-Based Vulnerabilities**
  - **Brute-Force** → this attack happens when the attacker uses system resources with trial-and-error, using an automatic word list of usernames & passwords.

---

## 4. Brute-Force Attacks

### 4.1 Brute-Forcing Usernames
- Usernames are often very easy to guess if the username has a recognized pattern, like an email address.
- Common pattern: `FirstName.LastName@Company.com`
- Sometimes high-level privileged users have obvious names like `"Admin"`, `"Administrator"`.

### 4.2 Brute-Forcing Passwords
- Depends on the strength of the password rules.
- Users can circumvent these rules but still make a memorable password:
  - `Password&Like` → `Password12`
  - `MyPassword12`

### 4.3 How the Web App Behaves During a Brute-Force Attempt
- Username enumeration: what the app allows you to observe about website behavior when you observe:
  - **Status Code** → if the probably will be the same for the most of your guesses — so any difference in the status code and the response message also is an important indicator.
  - **Error Message**:
    - Sometimes the error message is different depending on whether:
      - The username is valid → "invalid password"
      - The username is invalid → generic "invalid username or password"
    - Sometimes developers make a small typo interesting mistake — the response message that shows a lot for the hackers.

### 4.4 Pseudocode — Login Response Logic

```
if username in usernames:
    if password hash == password_hash(username):
        → "Login successful"
    else:
        → "Invalid password"
else:
    → "Invalid username or password"
```

**Lab 3 note:** Firstly check if the username is right (valid username or password, NOT "invalid username or password").
- If the username is right → go to perform some operations (hashing, etc.) on the password, and check if the password is right.
- But if the username is NOT right → on the password: hashing and this may confuse the attacker, so if attacker guesses password correctly but username is wrong → still `"invalid username, password"` → attacker won't know which one is correct.

---

## 5. Defending Against Brute-Force Attacks

Before we said there are two weaks attacks that use valid username to guess the password.

### 5.1 Account Lockout
- **Locking the account** that repeats failed attempts trying to access too many times.
- IP blocking → attacking user unavailable.

**But** this technique might still be vulnerable:
- If this technique still vulnerable to **IP Block or User unavailable** → to many attempts.
- But if the website blocks for a few minutes (e.g., "3 wrong attempts → go to 9 min lockout") — the attacker can just wait the lockout period and then continue guessing.

### 5.2 Rate Limiting
- Blocking IP instead of locking the account — avoiding account enumeration.
- But it's still not 100% secure, as the attacker can:
  - Change IP via Proxy / VPN
  - Manipulate the sequence/order between requests to avoid blocking
  - Send more than one request from this IP (can guess more than one password per request)

---

## 6. Case Study — Real Testing Scenarios

### Scenario 1: Web App Blocks After Two Wrong Attempts
- We saw before there are two weak attacks — as it will be able to guess the username and password used to brute force these attacks.
- We have 3 different scenarios here:
  1. **Blocking IP → Attacked** — block the other account, trying for ex. 2x website. Block U for 3 wrong attempts, so down any day 3 passwords on different accounts.
  2. **A lot of people use the same username(real looked) unfamiliar — a pass on a different website.**

### Scenario 2: Account Lockout Bypass via Headers
- As we said before this attack is unable to attack the account — we'll still
- Try changing **"X-Forwarded-For"** header — manipulate the security by sending the request from this IP you can bypass more than one password request.

> ⚠️ **Rate limiting:** if the sequency built on your password can guess more than one password or send more than one request from this IP (can guess more than one password per request).

### Scenario 3: Third Scenario — Account Enumeration via Technique
- Continue in account lockout technique.
- At the scenario we have here that the website is shown Ban message or Block when you enter 5 times wrong (= 5 password attempts).
- The generic message ("invalid username or password") for specific users here — so that means this username is valid. So I can easily use this username to Brute Force this using credentials.

### Scenario 4: Password Reset / Brute Force with Dataset
- **Intercepting Request:** the data set in JSON format like:
```json
{
  "username": "...",
  "password0": "...",
  "password1": "...",
  "password2": "..."
}
```
- So change password 5 times.

---

## 7. 2FA (Two-Factor Authentication)

This vulnerability arises **because** the developer accepts data sent without:
- Valid username/password without validation
- Graphical captcha where the user doesn't allow for sending too many queries in one request, and server performs them one bypass.

### 2FA → Secures websites and defends against 2 Factors to verify.
- You have your password, and sometimes (knowledge) and something you have like OTP.
- For 1st you'll see 5th you know (like your password), and sometimes an attacker on authenticate — you have both the password and OTP.

### 2FA is hard for an attacker:
- But not 100% — in this world would require 15 seconds of a 1-7% success rate.

---

## 8. 2FA Tokens

- Many high-end security websites deploy 2FA tokens on specific devices like RBA (Risk-Based Authentication), the Google Authenticator.

### Key Points
- The key feature in this technique that the code randomly and locally generated on the device, without being sent through the internet.

### A) TOTP / Authenticator Apps
- Time-based codes generated on the device locally — not transmitted over the internet.

### B) SMS
- Some websites support SMS verification code, but it's exposed to be hacked because of interception or SIM swapping.
- Log, mild, Brek, etc. — message logs can leak via SMS.

---

## 9. 2FA Lab — Bypass Techniques

**Lab (2FA bypass)**
1. Login with true credentials, develop then login process — some from initial credentials.
2. Some from initial credentials.
3. Go to the other cookies and omit other way (cookies and omit) — the same credentials.
4. Are logged in or not based on session cookies that are logged in — your cookies that set today.
5. Name 2 passwords only (my, not based) one username, password 2FA code.

**Technique BT different**
- Username = "..."
- Password1 = "123456"
- Password2 = "..."

**Note**: So change password with list of:
- "pass1", "pass0", "pass2"...

---

## 10. Cookies & Session Vulnerabilities

### 10.1 What are Cookies?
When you submit the website use this cookie — to determine who's account you're trying to access.
`Cookie: Carlos=Carlos`

### 10.2 Verification Code
`Verification_Code = 123456`

### 10.3 Cookie Tampering / Role Swapping
- In this case the attacker can login with their own credentials, but
- Then change the value of their own account cookie to another user's cookie — this is **very dangerous**, especially when the attacker is able to brute-force the 2FA code.
- They will be able to login without the password; even need to know the password.

---

## 11. Password Reset Vulnerabilities

### 11.1 Reset Password with Token
- Reset password within username or email.
- First, the website generates a token.
- Then a self about (or random) password.

### 11.2 Token Flow
```
TempToken = "<random_string>"
```
- First check if this token belongs to your reality, relating to this user ID.
- Check if this token to your reality (does it match the user's reset request?).
- Generate a short-term token and reset-link.
- Changing password maybe vulnerable to the same vulnerability, unless developer specifically handles it as a hidden reply, in the request.
- Login is not mandatory as with new password.
- I have the ability to change password without entering the old password.

### 11.3 Weak Reset Token Generation
- Carlos will be infected (victim) if the cookie document-location = `2xPort<Confirm='Carlos'...>`
- Carlos login (length of Carlos token login).
- Then edit the logs left.
- Sometimes the mistakes: 
  - If some password mechanism issue in the reset password workflow.
  - This is NOT a password-based authentication, user may fail to reset password and login the Reset Password.

### 11.4 Key Vulnerable Points in a Reset Flow
1. **Check the token you receive** exists / is valid.
2. **Check if this token** relates to this specific user — does the token belong to the right user?
3. **Check if the password** related to this username/password combination.
4. **Generate a short-term token** and Reset-link only (don't allow reuse).
5. Changing password may be vulnerable in how the dev implements it — do it as a hidden reply, specialized when the request.

### 11.5 Reset Password Weak Token Example
- If the Reset Password link with username
  - (e.g. the username = "...", password = "...")
  - in this lab 1 replaced my username with victim username, and I'm able to change their password.

> ⚠️ **Caution:** This is NOT a password-based authentication — generate a strong token.

---

## 12. Brute-Forcing Passwords — Successfully Changing Another User's Password (Lab Case)

3. **Successfully changed** password:
   - try to Brute-Force Carlos's and password new password.
   - But map → Blocksme.

4. Notice that there's no block if you're entering a new password different from Carlos's current password (invalid).
   - The current password is invalid.

5. There are two different errors:
   - The current password is invalid / confirm password mismatch.
   - The new password and confirm password don't match.

> So Brute-Force passwords with username with list of "pass0", "pass1", "pass2"...
> Search to know unknown passwords:
> If new pass is not the same of confirm pass → it is NOT the same confirm pass.
> So Brute Force passwords 3 and confirm Pass solved the web!

---

## Summary / Key Takeaways

- Authentication verifies **identity**; Authorization verifies **permissions**.
- Weak authentication = Broken Authentication → full account takeover risk.
- Always test:
  - Username enumeration via status codes / error messages
  - Brute-force protections (lockout, rate limiting, IP-based blocking) and their bypasses (X-Forwarded-For, proxies, request sequencing)
  - 2FA implementation (token generation method — local TOTP is safer than SMS)
  - Cookie-based session integrity (tampering / role swapping)
  - Password reset token validation (ownership check, expiry, single-use)
