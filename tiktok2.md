**Program:** Tiktok

**Title: Ability to Post Reply with Video to Comments from Private Accounts
## Description:

While testing TikTok’s **Reply with Video** feature, I noticed an **inconsistent privacy enforcement**:  

- When a video is made **private**, TikTok correctly blocks posting a reply video to comments on that video.  
- However, when the **creator switches their entire account to private**, TikTok **does not block replies**, allowing the video reply to be posted.  

I tested this as follows:  

1. I found a **public video** and used the **Reply with Video** feature on a comment.  
2. I recorded a reply and **saved it as a draft** without posting.  
3. I first made the **video itself private** — posting the draft reply was correctly blocked.  
4. I then switched the **creator’s account to private** — posting the same draft reply **succeeded**, and the reply overlay still displayed:  
   - the **comment text**  
   - the **commenter's name**  
   - the **reply reference to the video**  

This means that **non-followers of the private account can see content (comment + username + reply overlay) that should be restricted**, creating a privacy and low-integrity issue. TikTok enforces privacy at the **video level** but not at the **account level** for this feature.

---

## Steps to Reproduce
1. Find a public video on TikTok with a comment.  
2. Use **Reply with Video** on the comment by press holding the comment and selecting "Reply With Video"
3. record a video and save it as draft.  
4. Go to Account Settings and Privacy and Make the **creator’s account private**.  
5. As attacker Post the draft reply.  
6. Observe that Attacker Has successfully replied to the comment of vudeo they no longer have access to and Verify that a **non-follower account** can see the comment text, commenter name, and reply overlay.  

---

## Impact
- Exposes **comment text** from a private account  
- Exposes the **name** of the commenter  
- Shows the **reply overlay reference**  
- Allows posting a reply to content the user should no longer have access to  

This is a **privacy and low-integrity violation**, as users can interact with content they should no longer see.

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
| Integrity (I) | Low |
| Availability (A) | None |

---

## Notes / Additional Information
- TikTok **correctly blocks replies when a video is private**, showing the privacy check exists.  
- The bug occurs because **account-level privacy is not enforced** for the Reply with Video feature.  
- Could be exploited at scale if users save drafts and creators switch accounts to private.
