**Program:** Tiktok  **Asset:** 835599320  **Weakness:** Business Logic Error

**Title: Removed group member can access ongoing game and view updated participant list and scores
## Description:

When a user is added to a group and is later removed from the group, they lose access to the group as expected. However, the user can still access the previously initiated game instance and interact with it.

- Upon playing, the removed user can view the scores and identities of participants who played after their removal (unauthorized information disclosure).

- Additionally, the removed user’s score is later incorporated into the shared scoreboard and ranked among current participants, if they Play their turn before some members in the group. their own score later appear in the shared scoreboard once any participant plays their turn after the removed user plays.

This indicates that the game system relies on stale authorization and does not revalidate whether the user is still a member of the group before giving them access.

---

## Steps to Reproduce
1. Create or open a group for this test make sure to add 4 people (member 1-4)
   ![IMG_3068](https://github.com/user-attachments/assets/e87b16e7-44d3-4a97-aa3f-666a371eb4d6)
2. Start a game within the group (for example "Fruite Cutter Game")
   ![IMG_3065](https://github.com/user-attachments/assets/def76b27-45e1-4ef7-a69c-8ac0fa93e9cb)
![IMG_3066](https://github.com/user-attachments/assets/436a1fa3-7a44-465a-94e1-41adb9e38afe)

3. As the initiator of the game start the game and wait for other members to play their turn
 ![IMG_3074](https://github.com/user-attachments/assets/5f9061d3-e0ce-4474-921f-5e8cea4c9918)

4. As admin of the group, before anyone in the group plays their turn, remove one of the member (for this example **member 2**) before they play their turn by going to the member list and clicking the three dot and then click remove
![IMG_3067](https://github.com/user-attachments/assets/ec05c72e-9303-4ba2-8fc1-42bf44c7c56a)

5. after the member removal make sure **member 3** who is currently in the group has played their turn in the game and have recieved their score and ranking
6. (Before **member 4** plays) From the removed user’s account:

- Access the previously initiated game
![IMG_3071](https://github.com/user-attachments/assets/25b9517f-571a-4df3-b2d1-5ce90f19dfda)

- Click Play to play their turn
7. After playing their turn Observe that the removed user can:

- View the updated scoreboard with every member's score including **member 3** score (even though they were no longer in the group when they played their turn

- See participants of the game
![IMG_3059](https://github.com/user-attachments/assets/505df46a-9c21-4d9c-be21-67449fa5e9f1)

8. Now Observe that after the last member which is **member 4** plays their turn 

- The scoreboard in the group updates

- The removed user’s previously submitted score is now included

- The removed user is ranked among current participants based on their score


---

## Impact
A removed group member can keep viewing updated group game data and discover newly added group members (who joined after their removal) if those members participate in the game. And also have their gameplay results later incorporated into the shared scoreboard if they play before everyone in the group.


---

## Severity
**CVSS v3.0 Base Score:** 5.4 → Medium  

| Metric | Value |
|--------|-------|
| Attack Vector (AV) | Network |
| Attack Complexity (AC) | Low |
| Privileges Required (PR) | Low |
| User Interaction (UI) | None |
| Scope (S) | Unchanged |
| Confidentiality (C) | Low |
| Integrity (I) | low |
| Availability (A) | None |

---
