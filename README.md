<h1 align="center"> Jogo da Velha — Assembly 8086</h1>

> A complete Tic-Tac-Toe implementation in Assembly 8086 with PvP and PvC (AI) modes.

<p align="center">
  <img src="https://img.shields.io/badge/Assembly-8086-6E4C13?style=flat-square&logo=intel&logoColor=white" alt="Assembly 8086"/>
  <img src="https://img.shields.io/badge/License-MIT-22c55e?style=flat-square" alt="MIT License"/>
  <img src="https://img.shields.io/badge/Platform-MS--DOS%20%7C%20DOSBox-blue?style=flat-square" alt="Platform"/>
  <img src="https://img.shields.io/badge/Status-Complete-success?style=flat-square" alt="Status"/>
</p>

---

## Overview

This project is a fully functional Tic-Tac-Toe game written in pure Assembly 8086, running in text mode on MS-DOS (or DOSBox). It features two game modes: **Player vs Player** and **Player vs Computer** with a rule-based AI opponent.

The implementation explores low-level programming concepts including direct memory manipulation, register operations, stack management, and DOS/BIOS interrupts — all without any high-level abstractions.

---

## Features

- **Two game modes** — PvP (human vs human) and PvC (human vs AI)
- **Rule-based AI** — the computer opponent follows a priority strategy: win → block → center → first available
- **Input validation** — rejects invalid positions and occupied cells
- **Replay system** — play again without restarting the program
- **30+ procedures** — modular code organized with `CALL`/`RET` and stack preservation
- **980 lines** of pure Assembly 8086 with no external libraries

---

## How to Play

### Menu
```
JOGO DA VELHA - 8086
Escolha o modo de jogo:
1 - Jogador vs Jogador
2 - Jogador vs Computador
Opcao: _
```

### Board
```
   1   2   3
1    |   |
  -----------
2    |   |
  -----------
3    |   |
```

Enter the **row** (1-3) and **column** (1-3) to place your mark. Player X goes first.

---

## AI Strategy

The computer opponent (Player O) uses a deterministic priority-based algorithm:

| Priority | Action | Description |
|----------|--------|-------------|
| 1 | **Win** | Completes a line if two pieces are already placed |
| 2 | **Block** | Prevents the human from winning on the next move |
| 3 | **Center** | Takes the center cell (index 4) if available |
| 4 | **Fallback** | Picks the first empty cell on the board |

This guarantees the AI never loses when it moves first and always blocks immediate threats.

---
## 🏗️ Architecture

### Memory Model
```asm
.MODEL SMALL
.STACK 100h
```

### Data Segment
```asm
tabuleiro           DB 9 DUP(' ')   ; 3x3 board as a linear array
modo_jogo           DB 0            ; 1 = PvP, 2 = PvC
jogador_atual       DB 'X'          ; current player symbol
jogadas_realizadas  DB 0            ; move counter (draw at 9)
```

### Coordinate → Index Conversion
```asm
; index = (row - 1) × 3 + (col - 1)
DEC BL       ; row - 1
DEC BH       ; col - 1
MOV AL, BL
MOV CL, 3
MUL CL       ; AL = (row - 1) × 3
ADD AL, BH   ; AL = final index
```

### Key Procedures

| Procedure | Responsibility |
|-----------|---------------|
| `MAIN` | Game loop, menu, win/draw detection |
| `INICIALIZAR_JOGO` | Resets board and control variables |
| `MOSTRAR_TABULEIRO` | Renders the 3×3 grid to screen |
| `REALIZAR_JOGADA_HUMANO` | Reads, validates, and applies human input |
| `REALIZAR_JOGADA_COMPUTADOR` | Executes AI logic for computer moves |
| `TENTAR_COMPLETAR_LINHA` | Detects "2 pieces + 1 empty" pattern |
| `VERIFICAR_VITORIA` | Checks all 8 winning combinations |
| `TROCAR_JOGADOR` | Alternates between X and O |
| `LIMPAR_TELA` | INT 10h scroll-up screen clear |
| `IMPRIMIR_STRING` | INT 21h/09h string output |
| `LER_CARACTERE` | INT 21h/01h character input with echo |

---

## 🔌 Interrupts Used

| Interrupt | Function | Usage |
|-----------|----------|-------|
| `INT 21h / 01h` | Read char with echo | Player input |
| `INT 21h / 02h` | Print char | Board rendering |
| `INT 21h / 09h` | Print `$`-terminated string | UI messages |
| `INT 21h / 08h` | Read char without echo | Pause between turns |
| `INT 21h / 4Ch` | Terminate program | Exit |
| `INT 10h / 06h` | Scroll up | Clear screen |
| `INT 10h / 02h` | Set cursor position | Reposition after clear |

---

## 📂 Project Structure
```
jogo-da-velha-assembly/
├── src/
│   └── jogo_da_velha.asm    # Source code (980 lines)
├── .gitignore
├── LICENSE
└── README.md
```

---

## ⚙️ Compilation

### Requirements

- **MASM** or **TASM** assembler
- MS-DOS environment or **DOSBox** emulator

### With MASM
```
MASM jogo_da_velha.asm;
LINK jogo_da_velha.obj;
```

### With TASM
```
TASM jogo_da_velha.asm
TLINK jogo_da_velha.obj
```

### Alternative: VSCode + ASM Extension

1. Install the [ASM Code Lens](https://marketplace.visualstudio.com/items?itemName=maziac.asm-code-lens) extension
2. Configure MASM/TASM in your environment
3. Open `src/jogo_da_velha.asm`
4. Right-click → **Run ASM Code**

---

## ▶️ Execution

### MS-DOS
```
jogo_da_velha.exe
```

### DOSBox (modern Windows)
```
mount c c:\path\to\project
c:
jogo_da_velha.exe
```

> **Minimum requirements:** MS-DOS or compatible emulator · Intel 8086+ · 64 KB RAM

---

## 🧠 Concepts Applied

- **Conditional jumps** — `JE`, `JNE`, `JL`, `JG`, `JB`, `JA` for flow control
- **Subroutines** — 30+ procedures using `CALL` / `RET`
- **Stack operations** — `PUSH` / `POP` for register preservation
- **Memory addressing** — `SI`, `DI`, `BX` with `BYTE PTR`
- **8-bit arithmetic** — `MUL`, `ADD`, `DEC`, `INC`, `XOR` for index calculation
- **Segmentation** — `.MODEL SMALL` with `.DATA`, `.STACK`, `.CODE`

---

## 📚 References

- ABEL, Peter. *IBM PC Assembly Language and Programming*. 5th ed. Pearson, 2001.
- Intel Corporation. *8086 Family User's Manual*. 1979.
- NORTON, Peter; SOCHA, John. *Peter Norton's Assembly Language Book for the IBM PC*. Brady Books, 1986.

---

## 👤 Author

**Rafael Sanguini Colagrossi** — Software Engineering Student

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/rafaelsanguini)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rsanguini)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://rafaelsanguini.vercel.app)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:rafaelcolagrossi@gmail.com)

---

⭐ If this project was helpful, leave a star — it means a lot!
