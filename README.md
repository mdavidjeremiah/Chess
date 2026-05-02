# ♟️ David Jeremiah's Chess Engine

**A fully playable single-player chess game** built from scratch in a single HTML file — no libraries, no frameworks, no dependencies. Just pure vanilla JavaScript, HTML, and CSS, running in any modern browser instantly.

> 🌐 **Zero install. Open `index.html` and play.**

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Live Demo](#-live-demo)
- [Features](#-features)
- [How It Works](#-how-it-works)
- [Piece Encoding](#-piece-encoding)
- [Move Generation](#-move-generation)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Tech Stack](#-tech-stack)
- [Known Limitations & Roadmap](#-known-limitations--roadmap)
- [Author](#-author)

---

## 🔍 Overview

This project is a hand-rolled chess engine and board renderer built entirely inside a single `index.html` file spanning over 1,200 lines of code. It implements legal move generation for all six piece types, turn-based play between White (human) and Black (AI/second player), castling logic, pawn double-push from starting rank, and a responsive board that scales to any screen size.

All game logic — board state, move validation, piece rendering, click handling, and square highlighting — is written in vanilla JavaScript with zero external dependencies.

---

## 🌐 Live Demo

Host on GitHub Pages or open locally:

```bash
git clone https://github.com/mdavidjeremiah/Chess.git
cd Chess
open index.html   # macOS
start index.html  # Windows
```

Or deploy via **GitHub Pages**: Settings → Pages → Branch: `main` → `/ (root)` → Save.
Your live URL will be: `https://mdavidjeremiah.github.io/Chess/`

---

## Features

- **Complete Chess Rules** — Legal move generation for all six piece types: Pawn, Rook, Knight, Bishop, Queen, and King
- **Castling** — Both kingside and queenside castling supported for both colours, with rook-presence checks and king-in-check guards
- **Pawn Double Push** — Pawns on their starting rank can advance two squares if both squares ahead are clear
- **Pawn Capture Diagonals** — Pawns correctly capture diagonally only when an enemy piece occupies the target square
- **Legal Capture Highlighting** — Valid destination squares (including captures) are highlighted on piece selection
- **Responsive Board** — The board scales dynamically to fill the viewport on any screen size, from mobile to desktop
- **Unicode Chess Pieces** — Pieces are rendered using HTML Unicode chess symbols (♟ ♞ ♝ ♜ ♛ ♚) — no image assets required
- **Classic Board Colours** — Green/brown chequered pattern (`#c7d776` / `#815c36`)
- **Single File Deployment** — The entire game lives in one `index.html` — copy it anywhere and it works

---

## ⚙️ How It Works

### Board Representation

The board is modelled as a flat **64-element JavaScript array** (`values[]`), where index `0` is the top-left square (a8) and index `63` is the bottom-right (h1). Each element holds a single character representing the occupying piece, or `0` for an empty square.

```
Index layout (0–63, row-major):
 0  1  2  3  4  5  6  7    ← Row 8 (Black's back rank)
 8  9 10 11 12 13 14 15    ← Row 7 (Black's pawns)
...
48 49 50 51 52 53 54 55    ← Row 2 (White's pawns)
56 57 58 59 60 61 62 63    ← Row 1 (White's back rank)
```

### Piece Encoding

Two separate character sets are used — one for each colour — keeping collision detection simple:

| Char | White Piece | Char | Black Piece |
|------|-------------|------|-------------|
| `p`  | Pawn        | `o`  | Pawn        |
| `r`  | Rook        | `t`  | Rook        |
| `n`  | Knight      | `m`  | Knight      |
| `b`  | Bishop      | `v`  | Bishop      |
| `q`  | Queen       | `w`  | Queen       |
| `k`  | King        | `l`  | King        |

Checking if a square contains an enemy piece is then a simple `"prnbqk".indexOf(values[x]) >= 0` (for Black checking White) — no object lookups or class hierarchies needed.

### Move Generation

Two functions handle all move logic:

**`checkWhite(n, values)`** — computes all valid destination squares for the White piece at index `n`.

**`checkBlack(n, values)`** — computes all valid destination squares for the Black piece at index `n`.

Each function uses **iterative ray-casting** for sliding pieces (rook, bishop, queen) — walking along a direction until hitting the board edge, a friendly piece (stop, don't include), or an enemy piece (include, then stop). Knights use fixed offsets with file-wrapping guards. Kings use single-step adjacency checks.

### Castling

Castling state is tracked with three boolean flags:

```javascript
var ck  = false;  // Has the King moved?
var cr1 = false;  // Has the queenside Rook moved?
var cr2 = false;  // Has the kingside Rook moved?
```

Castling is offered as a valid king move (±2 squares) when all intermediate squares are empty, neither rook has moved, and the king has not moved. The rook teleports to its post-castle square when the king lands.

### Rendering

The board is built as 64 `<div>` elements, absolutely positioned using calculated pixel coordinates based on `window.innerWidth` and `window.innerHeight`. Each square gets its piece via `innerHTML` set to the corresponding Unicode HTML entity. A click listener on each square drives the two-phase select → move interaction.

---

## 🚀 Getting Started

No installation required.

```bash
# Clone
git clone https://github.com/mdavidjeremiah/Chess.git
cd Chess

# Open directly in your browser
open index.html
```

That's it. The game loads immediately — White always moves first.

**How to play:**
1. Click any White piece to select it — valid moves highlight on the board
2. Click a highlighted square to move the piece there
3. Black responds (or a second player takes over for Black)
4. Play continues until checkmate or resignation

---

## 📁 Project Structure

```
Chess/
└── index.html    # Entire game: HTML structure, CSS styling, and JS engine (1,255 lines)
```

The whole project is intentionally a single file — maximally portable and deployable anywhere.

---

## 🧰 Tech Stack

| Layer | Technology |
|-------|-----------|
| Markup | HTML5 |
| Styling | CSS3 (absolute positioning, inline styles) |
| Game Logic | Vanilla JavaScript (ES5-compatible) |
| Piece Rendering | Unicode HTML entities (`&#9812;` – `&#9823;`) |
| Dependencies | **None** |

---

## 🗺️ Known Limitations & Roadmap

The current version is a solid foundation. A few standard chess rules are not yet implemented:

| Feature | Status |
|---------|--------|
| All 6 piece moves | ✅ Implemented |
| Castling | ✅ Implemented |
| Pawn double push | ✅ Implemented |
| Check detection | ⏳ Planned |
| Checkmate / stalemate detection | ⏳ Planned |
| En passant | ⏳ Planned |
| Pawn promotion UI | ⏳ Planned |
| AI opponent | ⏳ Planned |
| Move history / notation | ⏳ Planned |
| Undo / takeback | ⏳ Planned |

Contributions welcome — see below.

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/check-detection`
3. Make your changes to `index.html`
4. Commit: `git commit -m "Add check detection and king safety validation"`
5. Push and open a Pull Request

Good first issues: pawn promotion modal, en passant capture, visual check indicator.

---

## 👤 Author

**David Jeremiah**
[@mdavidjeremiah](https://github.com/mdavidjeremiah)
Litmus Tech Solutions · Kampala Uganda

---

<div align="center">

Built with ♟️ and vanilla JS · © 2026 David Jeremiah

</div>
