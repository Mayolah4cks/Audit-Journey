**Program:** Tiktok  **Asset:** business.tiktok.com  **Weakness:** Improper Access Control

**Title: Authorization Bypass Allows Removed Members to Access Draft Content via Direct Edit URLs
## Description:

When a user is granted permissions to manage a connected TikTok account within a Business Center organization, they are able to access and edit draft content through the content management interface.

However, after the user's permissions are revoked, access control is only partially enforced. While the user is correctly blocked from accessing the main content management page, the application fails to enforce authorization checks on direct draft edit endpoints.

As a result, the removed user can still access draft content by revisiting previously saved edit URLs. This indicates that the backend does not properly validate user permissions when accessing specific draft resources.



---

## Steps to Reproduce
1. Log in to TikTok Business Center and create a new organization.

2. Connect a TikTok account to the organization by navigating to(https://business.tiktok.com/manage/accounts/tiktok-accounts?org_id=ORG_ID)

3. Add a new team member to the organization and Assign the team member: Standard role

4. As admin Go to (https://business.tiktok.com/manage/business-suite/content-management?org_id=ORG_ID) and add couple new video and save them as draft
<img width="959" height="416" alt="Screenshot 2026-03-19 231956" src="https://github.com/user-attachments/assets/e2f22c13-c182-4ac8-ab4a-664f331f3842" />

5. Next go to (https://business.tiktok.com/manage/accounts/tiktok-accounts?org_id=ORG_ID) and Grant the added team memeber all permission to manage the connected TikTok account
<img width="668" height="311" alt="Screenshot 2026-03-19 232457" src="https://github.com/user-attachments/assets/06fb9085-492a-404b-bb96-d9203071bf58" />
<img width="846" height="454" alt="Screenshot 2026-03-19 230334" src="https://github.com/user-attachments/assets/b0669631-c59e-47a0-92d7-6e260da24a18" />

6. Log in as the added team member (attacker)
   
7. Navigate to the content management page (https://business.tiktok.com/manage/business-suite/content-management?org_id=ORG_ID) and Go to the Drafts tab.
   <img width="808" height="449" alt="Screenshot 2026-03-19 225536" src="https://github.com/user-attachments/assets/c8fb66c0-535e-4fde-8a2b-6d2ac2610a92" />

8. Click “Edit Post” on any of the existing draft.
<img width="959" height="449" alt="Screenshot 2026-03-19 225700" src="https://github.com/user-attachments/assets/a9623d5a-0b94-49af-8224-989f24b0aee2" />
9. Copy the URL of the edit page (this is important — it contains the direct reference to the draft resource).
<img width="959" height="511" alt="Screenshot 2026-03-19 232934" src="https://github.com/user-attachments/assets/79a9de61-4ae1-47cb-9000-cee2664c1f3a" />
10. Log back in as the organization owner/admin.
11. Go back to (https://business.tiktok.com/manage/accounts/tiktok-accounts?org_id=ORG_ID) and Remove the team member’s permission to manage the TikTok account 
<img width="959" height="451" alt="Screenshot 2026-03-19 233205" src="https://github.com/user-attachments/assets/b26498ea-939c-4b39-a4bf-8f1fdcd9e18e" />
12. Log back in as the removed/downgraded team member.
13. Attempt to access the content management page again (https://business.tiktok.com/manage/business-suite/content-management?org_id=ORG_ID) and observe that access is correctly denied
14. Now paste and open the previously copied draft edit URL, Observe that The draft edit page loads successfully. The user can:

- View the draft video

- See captions and metadata

- See scheduled time

- Observe any new updates made after permission removal

---

## Impact
A removed team member can continue to access draft posts using previously saved edit URLs, even after their permissions have been revoked. This allows them to view unpublished videos, captions, and scheduling details without authorization.

Additionally, any changes made to the draft after the user’s access has been removed are still visible to them. This means the attacker can continuously monitor updates, edits, or newly modified content, leading to ongoing exposure of sensitive business information. 


---

## Severity
**CVSS v3.0 Base Score:** 7.6 → Medium  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | Low |
| Privileges Required (PR) | Low |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | High |
| Integrity (I) | low |
| Availability (A) | None |

---
