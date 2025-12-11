# CONFIG Object - Game Parameters Guide

Semua game mechanics dapat diatur melalui `CONFIG` object di top of `script.js`.

## Quick Reference

```javascript
// ==== PLAYER SETTINGS ====
CONFIG.STARTING_POINTS = 100;        // Poin awal setiap pemain
CONFIG.MAX_PLAYERS = 8;              // Maksimal pemain di game
CONFIG.MIN_PLAYERS = 2;              // Minimal pemain untuk mulai

// ==== MOVEMENT & SCORING ====
CONFIG.START_PASS_BONUS = 50;        // Bonus poin saat melewati START
CONFIG.WIN_SCORE = 500;              // Poin yang diperlukan untuk menang

// ==== JAIL MECHANICS ====
CONFIG.JAIL_PAYMENT_COST = 100;      // Biaya untuk keluar dari penjara
CONFIG.MAX_JAIL_TURNS = 3;           // Berapa giliran sebelum auto-release

// ==== FINE & PENALTY AMOUNTS ====
CONFIG.FINE_AMOUNT = 100;            // Denda jika jawaban salah di kota orang lain
CONFIG.WRONG_PURCHASE_PENALTY = 50;  // Penalti jika salah jawab saat membeli kota
CONFIG.CORRECT_ANSWER_BONUS = 100;   // Bonus poin untuk jawaban benar (umum)
CONFIG.CORRECT_DANA_UMUM_BONUS = 10; // Bonus tambahan untuk DANA UMUM question
CONFIG.WRONG_ANSWER_PENALTY = 50;    // Penalti untuk jawaban salah (umum)

// ==== GAME CONSTANTS ====
CONFIG.BOARD_SIZE = 24;              // Jumlah tile di board
CONFIG.CITY_COUNT = 10;              // Jumlah kota
CONFIG.BASE_CITY_PRICE = 100;        // Harga dasar kota pertama
CONFIG.CITY_PRICE_INCREMENT = 20;    // Kenaikan harga per kota berikutnya
```

## Contoh Modifikasi

### 1. Membuat Game Lebih Challenging

```javascript
// Pemain mulai dengan poin lebih sedikit
CONFIG.STARTING_POINTS = 50;

// Butuh poin lebih banyak untuk menang
CONFIG.WIN_SCORE = 1000;

// Denda lebih besar
CONFIG.FINE_AMOUNT = 150;
CONFIG.WRONG_PURCHASE_PENALTY = 75;
```

### 2. Membuat Game Lebih Mudah

```javascript
// Pemain mulai dengan poin lebih banyak
CONFIG.STARTING_POINTS = 150;

// Lebih mudah menang
CONFIG.WIN_SCORE = 300;

// Bonus lebih besar
CONFIG.CORRECT_ANSWER_BONUS = 150;
CONFIG.START_PASS_BONUS = 75;
```

### 3. Ubah Jail System

```javascript
// Jajil lebih mahal tapi lebih quick
CONFIG.JAIL_PAYMENT_COST = 50;
CONFIG.MAX_JAIL_TURNS = 1;

// Jail lebih long dan mahal
CONFIG.JAIL_PAYMENT_COST = 200;
CONFIG.MAX_JAIL_TURNS = 5;
```

### 4. Ubah City Pricing

```javascript
// Kota lebih murah
CONFIG.BASE_CITY_PRICE = 50;
CONFIG.CITY_PRICE_INCREMENT = 10;

// Kota lebih mahal
CONFIG.BASE_CITY_PRICE = 200;
CONFIG.CITY_PRICE_INCREMENT = 40;
```

### 5. Ubah Player Limits

```javascript
// Support lebih banyak pemain
CONFIG.MAX_PLAYERS = 12;

// Hanya duo play
CONFIG.MIN_PLAYERS = 2;
CONFIG.MAX_PLAYERS = 2;
```

## Dimana CONFIG Digunakan

| Parameter | Digunakan Di | Fungsi |
|-----------|-------------|--------|
| `STARTING_POINTS` | `startGame()`, `resetGame()`, `addPlayer()` | Set awal poin pemain |
| `MAX_PLAYERS` | `addPlayer()` | Batasan pemain maksimal |
| `MIN_PLAYERS` | `startGame()`, `removePlayer()` | Batasan pemain minimal |
| `START_PASS_BONUS` | `movePlayer()` | Bonus melewati START |
| `WIN_SCORE` | `nextPlayer()` | Kondisi menang game |
| `JAIL_PAYMENT_COST` | `showJailOptions()` | Biaya keluar penjara |
| `MAX_JAIL_TURNS` | `showJailOptions()` | Auto-release penjara setelah N turns |
| `FINE_AMOUNT` | `handleFineAnswerResult()` | Denda di kota orang |
| `WRONG_PURCHASE_PENALTY` | `handleBuyAnswerResult()` | Penalti beli kota salah |
| `CORRECT_ANSWER_BONUS` | `handleDanaUmumAnswerResult()`, dll | Bonus jawab benar |
| `CORRECT_DANA_UMUM_BONUS` | `handleDanaUmumAnswerResult()` | Bonus tambahan DANA UMUM |
| `WRONG_ANSWER_PENALTY` | Semua answer handlers | Penalti jawab salah |
| `BOARD_SIZE` | `movePlayer()`, `showParkingSelector()`, dll | Ukuran board (tiles) |
| `CITY_PRICE_INCREMENT` | `initializeCityPrices()` | Increment harga kota |
| `BASE_CITY_PRICE` | `initializeCityPrices()` | Harga dasar |

## Tips Modifikasi

### ✅ Best Practices
```javascript
// BAIK - Gunakan CONFIG untuk semua constants
if (player.points >= CONFIG.WIN_SCORE) { ... }

// KURANG BAIK - Hardcode magic numbers
if (player.points >= 500) { ... }

// BAIK - Adjust config untuk balancing
CONFIG.WRONG_ANSWER_PENALTY = 75; // Lebih punitive
CONFIG.CORRECT_ANSWER_BONUS = 150; // Lebih rewarding

// KURANG BAIK - Ubah di dalam function
// (sulit dicari dan maintain)
```

### Testing Parameter Changes
Setelah mengubah CONFIG:
1. Test game initialization dengan `Mulai Permainan`
2. Test player movement & passing START (bonus)
3. Test buying city (penalty jika salah)
4. Test landing di kota orang (fine)
5. Test jail (payment & dice roll)
6. Test win conditions (score & last player)

### Cara Balancing Game
1. **Terlalu mudah menang?** ↑ `WIN_SCORE`
2. **Terlalu sulit menang?** ↓ `WIN_SCORE` atau ↑ `CORRECT_ANSWER_BONUS`
3. **Terlalu banyak penalty?** ↓ `FINE_AMOUNT` atau ↑ `CORRECT_ANSWER_BONUS`
4. **Kota terlalu mahal?** ↓ `BASE_CITY_PRICE` atau ↓ `CITY_PRICE_INCREMENT`
5. **Jail terlalu punitive?** ↓ `JAIL_PAYMENT_COST` atau ↓ `MAX_JAIL_TURNS`

---

**Pro Tip:** Dokumentasi ini adalah guide komprehensif. Setiap parameter dalam CONFIG dijelaskan, sehingga mudah untuk experiment dengan different game mechanics! 🎮
