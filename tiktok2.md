**Program:** Tiktok  **Asset:** 835599320  **Weakness:** Imporper Access Control

**Title: Improper Access Control Allows Sharing of Private Account Comment to Story
## Description:

TikTok allows users to share comments from videos to their stories. Normally, the platform prevents users from sharing comments that belong to private accounts. However, this restriction can be bypassed due to improper validation timing.

If a story draft containing a comment is created while the commenter’s account is public, and the commenter later changes their account to private, the drafted story can still be published successfully. TikTok does not revalidate the commenter’s privacy status when the story is posted.

This results in the comment from a now-private account being exposed through the attacker’s story, even though TikTok normally prevents sharing such comments.
![Image](https://github.com/user-attachments/assets/51d7e9af-73b2-460b-bd23-a404127e8da7)

---

## Steps to Reproduce
1. Find a video with a comment from a user whose account is currently public.
2. Tap the option to share the comment to your TikTok story and make some changes to the video by repositioning the video and Save the story as a draft without publishing it. (As shown in the poc below)
   https://github.com/user-attachments/assets/66a1cb3b-3735-4ea9-9095-834110ae2703
3. Now commenter makes their account private, observe you can no longer share their comment on story.
   ![Image](https://github.com/user-attachments/assets/3e3a41b0-1336-4194-8866-6b6dcd232fa0)
4. As attacker Return to the drafted story.
   ![Image](https://github.com/user-attachments/assets/24729a51-e718-4902-856b-6637306f6d71)
5. Publish the story.
6. Observe that you've succefully shared the comment of a private account on your story


---

## Impact
An attacker can publish a comment from a private account to their story by drafting the story before the account becomes private. This bypasses TikTok’s restriction that normally prevents comments from private accounts from being shared to stories.


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
| Integrity (I) | low |
| Availability (A) | None |

---
