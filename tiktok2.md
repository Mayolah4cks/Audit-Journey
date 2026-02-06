**Program:** Tiktok

**Title:** Teen can bypass Restricted Mode by deactivating account
## Description:

When a parent enables Restricted Mode on a teen account, the teen is normally prevented from logging out or switching accounts. However, we found that the teen can deactivate their account, which allows them to bypass the restriction and log into another account.
## Steps to Reproduce

1. Set up Family Pairing on tiktok between a parent and a teen account
3. From the parent account, enable Restricted Mode for the teen
4. now as the teen go to the tiktok app and go to **Settings and Privacy** Then enter **Account**
5. Click Deactivate or delete account
6. go ahead to deactivate the account
7. now observe that the teen has access to login in to a new account

## Expected Behavior:
Deactivating the account should not allow the teen to bypass Restricted Mode. The teen should remain unable to log out or switch accounts while Restricted Mode is active. Deactivating the account should also be restricted.

## Actual Behavior:
Teen can deactivate the account and then log into another account, bypassing the logout and account-switching restrictions imposed by Restricted Mode.

## Impact:
Teen can bypass parental control and access accounts or content that should be restricted.

**weakness**: business logic flaw

**severity calculation**: AC: Low - PR: None - confidentiality: low - integrity: low - Scope: Unchanged
