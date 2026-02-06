**Program:** Tiktok

**Title:** Family Pairing (Web): Restricted Mode does not prevent teen from logging out or switching accounts
## Description:

Family Pairing is now available on the TikTok web app platform (tiktok.com) here https://www.tiktok.com/setting/family-pairing/. When a parent enables Restricted Mode for a teen account, the restriction is expected to prevent the teen from logging out or switching accounts. However, on the web version, the teen account can still log out and switch accounts, bypassing the intended restriction.
## Steps to Reproduce

1. Set up Family Pairing on tiktok web at https://www.tiktok.com/setting/family-pairing/ between a parent and a teen account
3. From the parent account, enable Restricted Mode for the teen
4. Log in to the teen account on tiktok.com (web
5. Click Log out
6. Observe that the action succeeds despite Restricted Mode being enabled

## Impact:
A teen can bypass Family Pairing Restricted Mode on TikTok Web by logging out or switching accounts, undermining parental controls intended to limit account access.
**weakness**: Improper access control

## Notes

- The restriction appears to be correctly enforced on mobile platforms

- The issue affects only the web implementation of Family Pairing Restricted Mode

**severity calculation**: AC: Low - PR: None - confidentiality: low - Scope: Unchanged
