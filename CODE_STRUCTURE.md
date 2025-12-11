# Monopoli Game - Code Structure Quick Reference

## File Organization

```
monopoli-master/
├── index.html                 # Game UI structure
├── styles.css                 # Game styling & layout
├── script.js                  # Game logic (1368 lines, well-organized)
├── REFACTORING_SUMMARY.md     # Complete refactoring documentation
├── CONFIG_GUIDE.md            # How to modify CONFIG parameters
├── CODE_STRUCTURE.md          # This file
└── SOAL_TEMPLATE.txt          # Question template reference
```

---

## script.js - 17 Organized Sections

### 1. GAME CONFIGURATION & CONSTANTS (Lines 1-33)
- `CONFIG` object dengan 16 game parameters
- Satu tempat untuk adjust game mechanics

**Jump to:** Cari `const CONFIG`

---

### 2. GAME STATE VARIABLES (Lines 34-69)
- `players[]` - array pemain aktif
- `jailState{}` - tracking pemain di penjara
- `gameRunning`, `gameStarted` - game flow flags
- `cityOwnership{}` - kepemilikan kota
- `usedQuestions[]` - tracking soal yang sudah digunakan

**Key Variables:**
```javascript
players[i] = {
    name: "Pemain X",
    points: 100,           // Poin/uang
    position: 0,           // Tile position
    color: "player-1",     // CSS color class
    hasAngelCard: false    // Status angel card
}
```

---

### 3. BOARD & CITY CONFIGURATION (Lines 70-123)
- `cities[]` - 10 nama kota
- `boardTiles[]` - 24 tile configuration
- `initializeBoardTiles()` - setup board
- `updateCityOwnershipDisplay()` - visual markers

**Tile Types:** `start`, `question`, `opportunity`, `community`, `parking`, `jail`

---

### 4. QUESTION SYSTEM (Lines 124-171)
- `allQuestions[]` - 70 ethics questions
- `getUnusedQuestion()` - get random unused question
- `resetUsedQuestions()` - reset untuk game baru

**Flow:** `getUnusedQuestion()` → mark as used → return

---

### 5. CITY SYSTEM (Lines 172-227)
- `initializeCityPrices()` - calculate harga setiap kota
- `updateCityOwnershipDisplay()` - update visual markers

**Pricing Formula:** `BASE_CITY_PRICE + (index * CITY_PRICE_INCREMENT)`

---

### 6. GAME LIFECYCLE (Lines 228-351)
- `initGame()` - setup saat page load
- `startGame()` - mulai permainan aktual
- `resetGame()` - reset untuk bermain lagi
- `clearLog()` - clear game log display
- `initJailState()` - setup jail tracking

**Game Flow:** `window.onload` → `initGame()` → User clicks "Mulai Permainan" → `startGame()`

---

### 7. BOARD & VISUAL SYSTEM (Lines 352-447)
- `createBoard()` - create tile DOM elements
- `createCardBoxes()` - create card display boxes
- `getBoardPosition()` - calculate grid position
- `createPlayerPieces()` - create player piece DOM

**Board:** 7x7 CSS grid dengan 24 tiles di perimeter

---

### 8. PLAYER MANAGEMENT (Lines 448-567)
- `addPlayer()` - tambah pemain baru
- `removePlayer()` - hapus pemain
- `eliminatePlayer()` - eliminate pemain (score ≤ 0)
- `checkEliminations()` - check semua pemain setiap turn

**Elimination Trigger:** `if (player.points <= 0)`

---

### 9. MOVEMENT & DICE SYSTEM (Lines 568-659)
- `rollDice()` - roll dengan animation
- `movePlayer()` - move with animation, handle START bonus
- `movePlayerToPosition()` - instant move (cards)

**START Bonus:** Trigger jika `oldPosition + steps >= BOARD_SIZE`

---

### 10. TILE EFFECT SYSTEM (Lines 660-852)
- `handleTileEffect()` - router untuk tile types
- `handleCityTile()` - handle city (buy/fine)
- `handleParking()` - handle parking (free movement)
- `showParkingSelector()` - modal untuk pilih posisi
- `handleJail()` - handle jail
- `showJailOptions()` - modal untuk jail (pay/roll)

**Main Router:** `switch (tile.type)` → call handler

---

### 11. CITY PURCHASE & FINE SYSTEM (Lines 853-950)
- `showBuyCityOption()` - show question untuk beli kota
- `showQuestionForFine()` - show question untuk bayar denda

**Question Setup:**
```javascript
currentQuestion = {
    question: selectedQuestion.q,
    answers: selectedQuestion.c,
    correct: selectedQuestion.a,
    cityName: cityName,
    price: price,          // For buy
    type: "buy" | "fine",  // Question type
}
```

---

### 12. QUESTION HANDLING (Lines 951-1066)
- `answerQuestion()` - main orchestrator
- `handleBuyAnswerResult()` - process buy answer
- `handleFineAnswerResult()` - process fine answer
- `handleDanaUmumAnswerResult()` - process community chest
- `handleDefaultAnswerResult()` - fallback

**Answer Types & Points:**
- Buy correct → deduct price, own city
- Buy wrong → -WRONG_PURCHASE_PENALTY
- Fine correct → pay nothing
- Fine wrong → -FINE_AMOUNT
- Dana Umum correct → +CORRECT_ANSWER_BONUS + CORRECT_DANA_UMUM_BONUS
- Dana Umum wrong → -WRONG_ANSWER_PENALTY

---

### 13. CARD SYSTEMS (Lines 1067-1191)
- `opportunityCards[]` - KESEMPATAN cards (8 total)
- `communityCards[]` - DANA UMUM cards (8 total)
- `handleOpportunity()` - landing di KESEMPATAN
- `handleCommunity()` - landing di DANA UMUM
- `executeOpportunityCard()` - apply effect
- `executeCommunityCard()` - apply effect

**Card Types:**
- `{msg, money, score}` - point/money effect
- `{msg, move}` - movement effect
- `{msg, goTo, bonus}` - go to tile + bonus

---

### 14. GAME FLOW & TURN SYSTEM (Lines 1192-1226)
- `nextPlayer()` - advance turn, check win conditions
- Win condition 1: `player.points >= CONFIG.WIN_SCORE`
- Win condition 2: `players.length === 1`

**Turn Flow:** Roll → Move → Handle Tile → Next Player → Check Win

---

### 15. UI UPDATE SYSTEM (Lines 1227-1289)
- `updateUI()` - update player stats, current player
- `updateGameInfo()` - update message display
- `logMessage()` - add timestamped log entry

**UI Updates:** Called after major game events

---

### 16. MODAL SYSTEM (Lines 1290-1362)
- `showCardModal()` - show card dengan random selection
- `closeCardModal()` - close dan clear modal
- Modals digunakan untuk: Questions, Cards, Jail, Parking

**Modals (dari HTML):**
- `#questionModal` - questions & parking
- `#cardModal` - cards (KESEMPATAN & DANA UMUM)

---

### 17. INITIALIZATION (Lines 1363-1368)
- `window.onload = initGame` - setup saat page load

---

## Function Name Patterns

### Handler Functions
`handle*()` - Respond to events
- `handleCityTile()`, `handleJail()`, `handleParking()`

### Show/UI Functions
`show*()` / `update*()` - Display & UI updates
- `showBuyCityOption()`, `updateUI()`, `showCardModal()`

### Initialize Functions
`initialize*()` / `init*()` - Setup & preparation
- `initGame()`, `initializeCityPrices()`, `initJailState()`

### Execute Functions
`execute*()` - Apply effects
- `executeOpportunityCard()`, `executeCommunityCard()`

---

## Data Flow Examples

### Player Buying a City
```
Roll Dice → Move → handleTileEffect()
→ handleCityTile() [if city not owned]
→ showBuyCityOption() [show modal with question]
→ User clicks answer
→ answerQuestion()
→ handleBuyAnswerResult()
→ [if correct & enough points] deduct points + update cityOwnership
→ updateCityOwnershipDisplay() [show marker]
→ nextPlayer()
```

### Player Landing in Jail
```
Move to tile 6 → handleTileEffect()
→ handleJail() [first time]
→ showJailOptions() [modal with pay/roll options]
→ User chooses:
   Option A: Pay → deduct JAIL_PAYMENT_COST → nextPlayer()
   Option B: Roll → if 6 → movePlayer() / else → turnsInJail++
   [After MAX_JAIL_TURNS → auto release]
```

### Community Card
```
Land on tile 4 or 10 → handleTileEffect()
→ handleCommunity()
→ showCardModal("DANA UMUM", communityCards)
→ Pick random card
→ User clicks "Terapkan Kartu"
→ executeCommunityCard(randomIndex)
→ Apply effect (move/points)
→ nextPlayer()
```

---

## Modification Quick Guide

### Add New Game Parameter
1. Add to `CONFIG` object (line ~20)
2. Use throughout code: `CONFIG.NEW_PARAM`
3. Example: `CONFIG.SPECIAL_BONUS = 25;`

### Add New Card Effect
1. Add to `opportunityCards[]` or `communityCards[]` (line ~1090)
2. Format: `{ msg: "...", effect: value }`
3. Handler will process automatically

### Add New Question Type
1. Duplicate handler function (e.g., `handleBuyAnswerResult()`)
2. Add to `answerQuestion()` switch
3. Set `currentQuestion.type = "newtype"`

### Add New Tile Type
1. Add to `boardTiles[]` (lines ~90-115)
2. Add case to `handleTileEffect()` switch (line ~680)
3. Create handler: `handleNewType()`

---

## Testing Checklist

- [ ] Game initializes correctly
- [ ] Can add/remove players
- [ ] Can start game
- [ ] Dice roll animation works
- [ ] Player movement animation works
- [ ] START pass bonus works
- [ ] City buying works (correct answer)
- [ ] City buying penalty works (wrong answer)
- [ ] Fine payment works
- [ ] Jail payment works
- [ ] Jail dice roll works (need 6)
- [ ] Jail auto-release after MAX_JAIL_TURNS
- [ ] Parking free movement works
- [ ] Opportunity/Community cards apply effects
- [ ] Player elimination triggers at score ≤ 0
- [ ] Win by score threshold
- [ ] Win by last player standing
- [ ] Game reset works

---

**Happy coding! 🎮✨**

Referensi ini akan membantu Anda navigate struktur code dengan cepat dan understand setiap section dengan jelas.
