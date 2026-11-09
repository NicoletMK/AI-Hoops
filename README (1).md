# AI Hoops

AI Hoops is a web-based game that introduces middle school students to Q-learning through basketball. Students first take shots themselves by setting an angle and a power. They then coach HoopsBot, a reinforcement learning agent, and watch its Q-table update after each training shot.

**Play it:** https://ai-hoops.web.app

This repository holds the version of AI Hoops described in:

> Nicole Tian, Ismaila Temitayo Sanusi, and Fred Martin. 2027. Playing to Win or Learning to Understand: Lessons from a Q-learning Game for Middle Schoolers. In *Proceedings of the 58th ACM Technical Symposium on Computer Science Education V.1 (SIGCSE TS 2027)*. https://doi.org/10.1145/3857222.3860216

## How the game works

Each level is a match between the student and the bot:

1. **Play.** The student takes five shots with an Angle slider (45–90°) and a Power slider (10–60).
2. **Coach.** The student trains HoopsBot with four buttons (Train 1, Train 10, Train 100, Train 1k). On each training shot the bot tries an angle-power combination and updates one Q-table cell.
3. **Compete.** HoopsBot takes five shots, each time choosing the cell it rates highest. The higher score wins.

The three levels grow the state space:

| Level | What changes | States | Q-table |
|---|---|---|---|
| 1 | Ball stays in one spot | 1 | One 10 × 10 page |
| 2 | Ball moves to a new spot each shot | 10 | A flipbook, one page per ball position |
| 3 | Ball moves and a blocker moves | 80 | A library, one page per ball and blocker position |

## Q-learning settings

- Update rule: `Q(s,a) ← Q(s,a) + α [r − Q(s,a)]`, with learning rate α = 0.2
- Reward: +1 for a basket, −1 for a miss
- Each shot is a single-step episode, so the update has no discount term
- Training uses an ε-greedy policy, with ε decaying from 1.0 to 0.05 (multiplied by 0.995 per training shot)
- In the match, the bot plays greedily

Q-table colors: green for a positive value, pink for a negative value, gray for an untried cell.

## Running it

The game is a single HTML file with no build step. Open it in any modern browser. It needs an internet connection to load fonts and the animation library from CDNs.

### Optional data logging

For research deployments, the game logs play to Firebase Firestore. If Firebase is not configured or cannot load, the game runs normally and saves nothing.

To log to your own project:

1. Create a Firebase project with Firestore.
2. Replace the `firebaseConfig` object in the HTML file with your project's config.
3. Set the Firestore security rules listed in the comment above `firebaseConfig`. They allow writes and block reads from the browser.

Each player gets a document in `users/{userId}` with these subcollections:

- `game_sessions`: one per completed level
- `shots`: one per student or bot shot
- `training_sessions`: one per Train button press
- `info_modal_views`: one per opening of the info panel

## Study materials

The `materials/` folder holds the blank pre- and post-surveys used in the study, with the intended answer for each concept item, and notes on how the sessions were run.

## License

- Code: MIT License (see `LICENSE`)
- Survey materials in `materials/`: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)

## Citation

If you use AI Hoops, please cite the paper above and this software release.

## Acknowledgments

This material is based upon work supported by the U.S. National Science Foundation under Grant No. 2112633.
