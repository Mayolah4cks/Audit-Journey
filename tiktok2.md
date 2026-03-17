**Program:** Tiktok  **Asset:** 835599320  **Weakness:** Iproper Access Control

**Title: Removed admin-added group member can bypass invite restrictions and fully rejoin group
## Description:

When a group is created, the admin can add members directly either **at creation** or **later manually**. Once added, users can participate in the group normally.  

**Observed Bug Behavior:**

If an admin adds a user at group creation (e.g., User A) and later removes them, all intended restrictions to prevent re-entry fail. Specifically:

1. Admin attempts to enforce restrictions by:  
   - Generating a new invite link (which should revoke old links)  
   - Disabling all invite links  
   - Requiring admin approval for join requests  

2. Despite these measures, User A can still rejoin the group using the **original invite link issued at creation**.  

3. Upon rejoining, User A gains **full privileges**, including:  
   - Viewing all existing messages  
   - Sending new messages  
   - Seeing all current and newly added group members  

4. These restrictions only work for users added by **non-admin members**, not for **admin-added users**.  

This demonstrates a **broken access control / privilege bypass**, where the system continues to trust prior membership or admin-added status instead of enforcing **current authorization**. Removed users can regain full access without any additional user interaction.



---

## Steps to Reproduce
1. Create a group as an admin.
   ![IMG_3068](https://github.com/user-attachments/assets/4b80bf7e-109f-410e-a790-6381afda3303)

2. Add a user (User A) directly at group creation (make sure you and the user are not friends so and invite can be sent to them) and click create
![IMG_3085](https://github.com/user-attachments/assets/94ac4e85-4fb8-48f2-b631-29b7538f1079)

3. As User A use the invite sent to join the group
   ![IMG_3086](https://github.com/user-attachments/assets/7bc8f93c-23a1-4153-be1b-5f6240dd5ad8)

4. As admin of the group Remove User A from the group.

5. Attempt to restrict their access using:

- Generate a new invite link (which should revoke old invites)
![IMG_3088](https://github.com/user-attachments/assets/e3962c75-79bf-4d34-8ddb-c9a437b11319)

- Disable invite links
![IMG_3089](https://github.com/user-attachments/assets/b438722b-f4f8-4b46-9445-048abb5001ba)

- Enable Require admin approval to join
  ![IMG_3090](https://github.com/user-attachments/assets/5a5b718f-af64-4114-bc74-d7c1d7aa15c5)

6. Have User A use the original invite link from when they were added at creation.
   
7. Observe that User A:

- Rejoin the group
  
- Can send messages freely

- Can see all current and newly added group members

---

## Impact
A removed admin-added member can bypass all group access restrictions and regain **full privileges**, resulting in:

- **Confidentiality breach:** full access to messages and group members  
- **Integrity breach:** ability to send messages and affect group state  
- **Loss of admin control:** Admin has no effective control over their access.

Restrictions only work for users added by **non-admin members**, not for **admin-added users**.  


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
