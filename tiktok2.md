**Program:** Tiktok  **Asset:** 835599320  **Weakness:** Business Logic Error

**Title: Reply with Video Drafts Can Expose Comments and Usernames from Deactivated Accounts
## Description:

While testing the **Reply with Video** feature on TikTok, I discovered that it is possible to publish a reply video referencing a comment from a user who has since **deactivated their account**.

When a reply video is drafted before the commenter deactivates their account, the draft retains the **comment text and the original username** in the reply overlay. If the commenter later deactivates their account and their comment becomes unavailable on the platform, the drafted reply video can still be published.

As a result, the published video continues to display the **original username and comment text** in the reply overlay, even though the source comment and account are no longer accessible on TikTok.

This behavior applies both to:
- comments made by the user **under their own videos**, and
- comments the user made **under any other videos across the platform**.

If a reply video draft exists for those comments, it can still be published after the account is deactivated, preserving the **username and comment text** in the reply overlay.

During testing, I also observed that TikTok correctly prevents this action when a **video is made private** or when an **account is switched to private**. In those cases, attempting to publish the drafted reply video results in an error indicating that the video or comment source no longer exists.

However, when the account is **deactivated**, the system does not perform the same validation and still allows the reply video to be published.

---

## Steps to Reproduce
1. Account A posts a comment on any TikTok video.
2. Account B selects the comment and uses the **Reply with Video** feature.
3. Account B records a reply video and saves it as a **draft**.
4. Account A **deactivates their TikTok account**.
5. Confirm from a third account that the **original comment is no longer visible on the platform**.
6. Account B publishes the drafted reply video.
7. The reply video is successfully posted and still displays:
   - **@original_username**
   - **the original comment text** in the reply overlay.
![IMG_2891](https://github.com/user-attachments/assets/70f9d237-214d-4060-a68a-cdee8a050783)


---

## Impact
This behavior allows comments and usernames from **deactivated accounts** to remain publicly visible through previously drafted reply videos.

Even though the original comment and account are no longer accessible on the platform, the reply video still preserves and exposes this information to other users viewing the video.


---

## Severity
**CVSS v3.0 Base Score:** 6.4 → Medium  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | Low |
| Privileges Required (PR) | None |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | Low |
| Integrity (I) | None |
| Availability (A) | None |

---
- The bug occurs because **account-level privacy is not enforced** for the Reply with Video feature.  
- Could be exploited at scale if users save drafts and creators switch accounts to private.
