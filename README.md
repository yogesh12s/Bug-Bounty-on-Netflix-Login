# 🔐 Netflix Session Hijacking Vulnerability – Responsible Disclosure

## 📌 Overview

This repository documents a security vulnerability discovered in the Netflix web platform that allows an attacker to hijack a user's session and gain full account access **without needing a username or password**, by reusing stolen session cookies.

> ⚠️ This vulnerability was discovered during ethical testing on my own Netflix account and has been responsibly reported to Netflix.

---

## 🧪 Vulnerability Summary

Netflix uses session cookies (such as `NetflixId` and `SecureNetflixId`) to manage user authentication across browsing sessions. During testing, I found that:

- These cookies can be **copied from a logged-in session** and **reused on another device or network**.
- The session remains fully valid — no re-authentication is required.
- No alert or challenge is triggered, even when reused from a **different IP address, user-agent, or device**.

This behavior enables a form of **session hijacking**, where an attacker with access to session cookies can impersonate the victim completely.

---

## 🎯 Affected Platform

- **Domain**: `https://www.netflix.com`
- **Environment**: Web (tested on Chrome)
- **Cookies involved**:  
  - `NetflixId`  
  - `SecureNetflixId`

---

## 🔁 Steps to Reproduce (PoC)

> ✅ This was performed only on my own account, using two devices I control.

1. **Login to Netflix** on Device A.
2. Open browser dev tools → Application → Cookies.
3. Copy the values of:
   - `NetflixId`
   - `SecureNetflixId`
4. On **Device B** (different browser or network):
   - Navigate to `https://www.netflix.com`
   - Open dev tools → manually set the copied cookies.
5. Refresh the page.

### ✅ Result:
- The user is immediately logged in.
- All account access is granted: viewing profiles, playback, settings, etc.
- No 2FA, no warning, no session invalidation.

---

## 🔥 Impact

If an attacker can steal a user’s session cookies through common techniques like:

- Cross-Site Scripting (XSS)
- Malware or browser extensions
- Insecure Wi-Fi networks
- Physical device access

Then the attacker can:

- Gain full control of the account
- Access personal viewing data
- Change profiles, settings, or plan
- Persist their access without detection

---

## 🛡️ Recommended Mitigations

Netflix (or any platform handling sensitive session data) should consider implementing the following:

- **IP Address / Device Fingerprinting Binding**  
  Limit session reuse from unknown IPs or devices.

- **Session Invalidation / Rotation**  
  Rotate session tokens frequently and invalidate old ones on suspicious activity.

- **Re-authentication Triggers**  
  Prompt for password or 2FA when session is reused in new environments.

- **Shorter Cookie Lifetimes**  
  Reduce the lifespan of session cookies to minimize risk.

---

## 🤝 Disclosure

- **Tested on**: Personal Netflix account  
- **Date of Discovery**: [15-01-2025]  
- **Reported to Netflix via**: [HackerOne / Email / Support Form]  
- **Disclosure Type**: Responsible Disclosure



## 📄 Disclaimer

This repository is for **educational and ethical purposes only**. Testing was conducted on my own account. Do not attempt to exploit or test vulnerabilities on systems you do not own or have permission to test.

