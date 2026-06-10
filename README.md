# 🐾 Paws & Panics

A minesweeper-style console game built in C++ using Object-Oriented Programming.

> Sausy is a small hungry dachshund. Help her find all the sausages — without getting struck by lightning.

---

## Video Demo

![Gameplay](demo.gif)

---

## Description

Paws & Panics is a minesweeper-style game played entirely in the Windows console. You control Sausy, a small hungry dachshund searching for sausages hidden across a grid called *Snackfield*. The ground is dangerous — lightning is buried everywhere. Dig carefully, use the hints, and find every sausage before Sausy runs out of hearts.

The project was built as an introduction to **Object-Oriented Programming** in C++. The original procedural code was refactored into four classes — `UI`, `Player`, `Snackfield`, and `Game` — each responsible for a distinct part of the program.

## 🎮 How to Play

1. Choose a map size between **4×4** and **10×10** (rows must be ≤ columns)
2. Enter a position `(row column)` to dig a tile
3. Find all the sausages before losing all your hearts

### Tiles

| Symbol | Meaning |
|--------|---------|
| `?` | Unvisited tile |
| `S` | 🌭 Sausage — collect all of these to win |
| `B` | 🦴 Bone — gives you +1 heart |
| `L` | ⚡ Lightning — lose 1 heart |
| `0–9` | Number of sausages in adjacent tiles |

### Hearts

- You start with **3 hearts**
- Hitting lightning costs **1 heart**
- Finding a bone gives **+1 heart**
- Lose all hearts → **Game Over**

---

## 🏆 Win & Lose Conditions

| Condition | Result |
|-----------|--------|
| All sausages found | ✅ You Win |
| Hearts reach 0 | ❌ Game Over |

---

## 🗂️ Project Structure

```
PawsAndPanics/
├── main.cpp          # Entry point — starts the game
├── Game.h / .cpp     # Game loop, input handling, tile processing
├── Snackfield.h/.cpp # Map grid, tile placement, nearby counting
├── Player.h / .cpp   # Hearts, sausage & bone tracking
└── UI.h / .cpp       # Colors, text animations, story text
```


### Design Decisions

**Why four classes?**
The original code was one long `main()` function with free-floating global variables. Splitting it into four classes made each responsibility explicit: `Snackfield` owns the map data so nothing else can corrupt it, `Player` owns the game state so score and hearts are never accidentally modified from the wrong place, `UI` is purely static because it holds no data of its own, and `Game` acts as the coordinator that connects them all.

**Why is `field` a pointer in `Game`?**
`Snackfield` needs the grid size to construct itself, but the size comes from user input *after* `Game` is created. Using `Snackfield* field` lets `Game` be constructed first with `field = nullptr`, then build the actual `Snackfield` object once the player has chosen a size. The destructor cleans it up with `delete field`.

**Why are `UI` methods static?**
`UI` holds no data — it only performs output. Making every method `static` means callers never need to create a `UI` object, which would be meaningless. It also signals clearly to a reader that `UI::typeText()` is a utility, not something tied to a particular instance.

**Why does `countNearby()` live in `Snackfield` and not `Game`?**
The nearby count requires reading the raw grid, which is private to `Snackfield`. Keeping that logic inside `Snackfield` preserves encapsulation — `Game` only needs to ask for the result, not reach into the data itself.


### OOP Concepts Used

| Class | Concept |
|-------|---------|
| `UI` | Static methods — no object needed |
| `Player` | Encapsulation — private data, public getters |
| `Snackfield` | Constructor with parameters, private helper methods |
| `Game` | Composition — owns a `Player` and a `Snackfield` |

---

## ⚙️ Requirements

- Windows OS
- g++ / MinGW (GCC 6.3.0 or newer)

---

## 🔨 How to Compile & Run

### Step 1 — Open CMD in the project folder

In File Explorer, click the address bar, type `cmd`, and press Enter.

### Step 2 — Compile

```bash
g++ main.cpp Game.cpp Snackfield.cpp Player.cpp UI.cpp -o PawsAndPanics.exe
```

### Step 3 — Run

```bash
PawsAndPanics.exe
```

---

## 📝 Notes

- This project was made as an introduction to OOP in C++
- This game uses Windows-only APIs (`windows.h`, `Sleep`, `SetConsoleTextAttribute`) and will only compile and run on Windows
- Map size input: rows must be between 4–10, columns must be ≥ rows
