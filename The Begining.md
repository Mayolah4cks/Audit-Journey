**Program:** Tiktok  **Asset:** 835599320  **Weakness:** Business Logic Error

**Title: Non-Subscribers Can Access Full Audio of Subscriber-Only Videos via “Use Sound” Feature
## Description:

TikTok allows creators to publish **subscriber-only videos** that can only be fully accessed by users who subscribe to the creator. Non-subscribers are supposed to see only a **short preview** of the video. After the preview ends, the video becomes blurred and the audio stops.

However, during the preview period TikTok still allows users to tap the **“Use Sound”** option attached to the video. When a non-subscriber records a video using that sound, the **entire audio from the subscriber-only video continues playing until the end**, allowing the attacker to listen to the full content without subscribing.

This effectively bypasses the intended subscription restriction for subscriber-only content.

# Context and Impact on Content Creators
For many TikTok creators, **audio is the most important part of the content**. A large portion of subscriber-only videos are:

- **Story-time content**
- **News or commentary**
- **Exclusive announcements**
- **Promo codes or discount codes shared only with subscribers**

In these types of videos, the **message is delivered entirely through speech**, meaning the video visuals are not necessary to understand the content.

For example, creators such as:  
https://www.tiktok.com/@cristinabruno1111/

primarily produce **story-time and commentary style content**, where the main value of the content is the **spoken narrative**. Even without seeing the video, a user can fully understand the information simply by listening to the audio.

Because of this vulnerability, an attacker can listen to the **entire spoken content of subscriber-only videos**, including exclusive information such as subscriber-only promo codes, announcements, or stories that were intended to be restricted to paying subscribers.


---

## Steps to Reproduce
1. Find a content creator with sub only content for example lets use (https://www.tiktok.com/@cristinabruno1111/)
2. go to their page and tap the subscription button
![Image](https://github.com/user-attachments/assets/abdd3e89-85a6-4690-813d-5030046f13dc)
3. Now click the list of sub only content
  ![Image](https://github.com/user-attachments/assets/284f0491-c053-458f-9acd-520513e6e2a4)
4. You will see the list of sub only content and also you will observe you can preview some of the videos for few seconds Now click one of the videos to preview it
  ![Image](https://github.com/user-attachments/assets/f434777c-3a37-4262-bee8-3e8e2a8a9174)
5. During the preview period (before the video becomes blurred), tap sound icon
   ![Image](https://github.com/user-attachments/assets/3b3decae-0be7-4413-99f8-1247cfbd48ca)
6. tap **“Use Sound.”**
   ![Image](https://github.com/user-attachments/assets/91ab2b87-d109-4580-9797-5b3f5aefa396)
7. select 10mins and start recording
   ![Image](https://github.com/user-attachments/assets/b63711d5-6fb5-4a7d-8ec1-da1741719e0e)
8. Observe that the **full audio of the subscriber-only video continues playing until the end**.
   ![Image](https://github.com/user-attachments/assets/8a6a7151-c0c8-4345-beab-01872104e472)

---

## Impact
A non-subscriber can listen to the full audio of subscriber-only videos using the **“Use Sound”** feature, bypassing the subscription restriction and exposing exclusive subscriber content.

Because the full audio track of the subscriber-only video is accessible, an attacker can obtain the complete spoken content of the restricted video. For many creators, the audio contains the entire message of the content, meaning the attacker can access all information intended only for subscribers.

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
| Confidentiality (C) | High |
| Integrity (I) | none |
| Availability (A) | None |

---
