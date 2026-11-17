# Crystal Circuit – handover notes

Single-file game: everything lives in `index.html` (plain JS + canvas, no build step).

## Releasing an update
1. Change `GAME_VERSION` near the top of the script **and** `BUILD` in the `kg-update` script at the bottom.
2. Put the same number in `version.json`. Players then get a "new version" notice.
3. Only if icons or the manifest change: bump `VERSION` in `sw.js`.
4. Add a changelog entry to `README.md`, and a news post on the hub (`kralnu90.github.io/news.json`).

## Where things are (sections in the script)
1. **DATA** – the tables to tune: `FATE` (D12 outcomes), `TIERS`, `BOSSR` (boss rounds), `ENEMY`, `CLASSES`, `GEMS`, `WEAPONS`, `ARMOUR`, `ITEMS`, `ACC`, `CARDS`, `RDECKS` (rival decks), `DIFF`, `BOARDS`.
2. **STATE & SAVE** – hero roster in localStorage key `crystal-circuit-heroes` (`{slots:[{p,m}],cur}`); `migrate()` upgrades old saves.
3. **BOARD & MATCH FLOW** – `newMatch`, `doRoll`, `restOfRound`, `finishMatch`.
4. **FATE, RIVALS, BOSS ROUNDS** – `fate`, `aiTurn`, `aiLand`, `simFight` (rival fights resolved behind the scenes).
4b. **CARDS ENGINE** – hands, `playCard`, `hostile` (Wary/Ward/Mirror), `chooseTarget` (grudges), traps, `passBuck`.
5. **BATTLE** – real-time arena: `startBattle`, `hitEnemy`, `hurtPlayer`, enemy AI in `upEnemies`.
6. **MENUS / SHOP / FORGE** – `renderMenu`, `rollStock`, `ACT` (all button actions), `renderTitle`.
7. **RENDER & LOOP**.

## Design rules agreed so far
- Defeat: back to the Sanctuary, −25% gil, −20% distance. No auto-revive items.
- Cards: 15-card owned deck, hand of 5, refill only what you used, 2 plays + 1 discard a turn. Vendor cards are temporary for the circuit. Traps are hidden; owners are immune. Dice cards are locked in before the roll.
- Rivals target by personality (Wildcard, Opportunist, Vengeful) using grudges; Wary limits hostile cards to one per target per round.
