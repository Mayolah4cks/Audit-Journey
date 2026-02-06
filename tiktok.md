**Program:** Tiktok

**Title:** Removed Organization Members Can Still Add Test Users to Mini Game Apps via Backend API 
## Description:

When a user is removed from a developer organization, they lose portal access. However, they can still add test users to Mini Game apps via the backend API using a previously obtained app client_key and any/their valid developer_id.

This occurs because the backend does not validate that the user is still a member of the organization. As a result, ex-members retain privileges they should have lost, allowing unauthorized modification of app test user lists.

## Steps to Reproduce

1. Create an organization on TikTok Developers
2. Creat an organization based app
   <img width="959" height="449" alt="Screenshot 2026-02-06 133601" src="https://github.com/user-attachments/assets/898695d9-18e9-4931-ae17-56ec5cc538ef" />

3. after that create a Mini Game app.
   <img width="959" height="449" alt="Screenshot 2026-02-06 133631" src="https://github.com/user-attachments/assets/18cef4fa-a1d5-40ee-8b7d-644f3b7ec532" />

4. Add a team member to the organization.
5. observe that the team member has access to the mini game app portal and can also see list of test user
   <img width="959" height="451" alt="Screenshot 2026-02-06 133229" src="https://github.com/user-attachments/assets/de631eb9-65f5-44fa-9d72-39b87a27e3ac" />
6. As the added team member save tha mini game app client key somewhere
   <img width="959" height="448" alt="Screenshot 2026-02-06 134030" src="https://github.com/user-attachments/assets/89de6ceb-903c-438e-acb7-6add40b10142" />
7. Now as Organization admin remove the team member from the organization.
8. As the removed team member observe that they no longer have access to the org or the mini game app portal
9. Now as the removed team member Using the previously obtained client_key and a/their valid developer_id, the removed member can send the following request to add test users or add themselves as test users:
```
POST /tiktok/v2/oauth/client/targetuser/add/ HTTP/2
Host: www.tiktok.com
Cookie: [revoked]
Content-Length: 70
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
Sec-Ch-Ua: "Not(A:Brand";v="8", "Chromium";v="144", "Google Chrome";v="144"
Content-Type: application/json
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Origin: https://www.tiktok.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/auth/tt4d?client_key=mgxvucj5n18h4leo&developer_id=7602942719925945352&app_name=GOD&app_id=7603690821040375820&for_rollout_target_user=true&lang=en-GB
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"client_key":"<redacted_client_key>","developer_id":"<redacted_developer_id>"}
```
10. The request succeeds, and as the organization admin observe that the test user is added to the list of test users for the mini game app

## Impact:
A removed organization member can still add test users to a Mini Game app, allowing unauthorized access to an organization app 
  
**weakness**: Improper access control

**severity calculation**: AC: Low - PR: None - confidentiality: low - integrity: low - Scope: Unchanged
