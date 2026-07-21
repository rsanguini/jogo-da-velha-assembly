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




## Como Jogar

### 1. Tela inicial

Ao executar, aparece o menu de seleção de modo:

```
JOGO DA VELHA - 8086

Escolha o modo de jogo:
1 - Jogador vs Jogador
2 - Jogador vs Computador
Opcao: _
```

Digite `1` ou `2` e pressione Enter.

### 2. O tabuleiro

```
   1   2   3
1    |   |
  -----------
2    |   |
  -----------
3    |   |
```

Os números no topo indicam as **colunas** e os da lateral indicam as **linhas**.

### 3. Realizando uma jogada

A cada turno, o jogo solicita:

```
Digite a linha (1-3): _
Digite a coluna (1-3): _
```

Exemplo — para jogar no centro:
- Linha: `2` → Coluna: `2`

```
   1   2   3
1    |   |
  -----------
2    | X |
  -----------
3    |   |
```

### 4. Final de partida

Ao terminar, é exibida a mensagem de vitória ou empate, seguida da opção:

```
Jogar novamente? (S/N): _
```

---

## Inteligência Artificial (modo PvC)

No modo Jogador vs Computador, o computador joga como `O` e segue esta ordem de prioridade:

1. **Vencer** — completa a própria linha se tiver duas peças seguidas
2. **Bloquear** — impede a vitória do jogador humano
3. **Centro** — ocupa a posição central (índice 4) se disponível
4. **Primeira livre** — joga na primeira célula vazia encontrada

---

## Arquitetura e Módulos

O programa usa o modelo `.MODEL SMALL` e está organizado em procedimentos independentes:

| Procedimento | Descrição |
|---|---|
| `MAIN` | Fluxo principal: menu, loop de jogo, condições de término |
| `INICIALIZAR_JOGO` | Reseta o tabuleiro (9 espaços) e as variáveis de controle |
| `MOSTRAR_TABULEIRO` | Renderiza o grid na tela substituindo marcadores pelos valores do array |
| `REALIZAR_JOGADA_HUMANO` | Lê linha/coluna, valida entradas e marca a posição no tabuleiro |
| `REALIZAR_JOGADA_COMPUTADOR` | Executa a lógica de IA para escolher a melhor jogada |
| `TENTAR_COMPLETAR_LINHA` | Detecta padrão "2 peças + 1 espaço" para vencer ou bloquear |
| `VERIFICAR_VITORIA` | Verifica as 8 combinações possíveis de vitória |
| `TROCAR_JOGADOR` | Alterna o símbolo ativo entre `X` e `O` |
| `IMPRIMIR_STRING` | `INT 21h / 09h` — imprime string terminada em `$` |
| `LER_CARACTERE` | `INT 21h / 01h` — lê um caractere com eco |
| `LIMPAR_TELA` | `INT 10h / 06h` — scroll up + reposiciona cursor em (0,0) |
| `PAUSAR` | `INT 21h / 08h` — aguarda tecla sem exibir na tela |

### Segmento de dados

```asm
tabuleiro           DB 9 DUP(' ')   ; array linear simulando a matriz 3x3
modo_jogo           DB 0            ; 1 = PvP, 2 = PvC
jogador_atual       DB 'X'          ; símbolo do turno atual
jogadas_realizadas  DB 0            ; contador de jogadas (empate em 9)
```

### Conversão coordenadas → índice linear

```asm
; índice = (linha - 1) × 3 + (coluna - 1)
DEC BL       ; linha - 1
DEC BH       ; coluna - 1
MOV AL, BL
MOV CL, 3
MUL CL       ; AL = (linha-1) × 3
ADD AL, BH   ; AL = índice final
```

---

## 🔌 Interrupções Utilizadas

| Interrupção | Função | Uso |
|---|---|---|
| `INT 21h / 01h` | Lê caractere com eco | Entrada do jogador |
| `INT 21h / 02h` | Imprime caractere | Renderização do tabuleiro célula a célula |
| `INT 21h / 09h` | Imprime string (`$`-terminated) | Mensagens de interface |
| `INT 21h / 08h` | Lê caractere sem eco | Pausas entre jogadas |
| `INT 21h / 4Ch` | Termina programa | Encerramento |
| `INT 10h / 06h` | Scroll up (limpar tela) | Limpeza de tela a cada turno |
| `INT 10h / 02h` | Posiciona cursor | Reposiciona após limpar tela |

---

## Conceitos de Assembly Aplicados

- **Saltos condicionais** — `JE`, `JNE`, `JL`, `JG`, `JB`, `JA` para controle de fluxo
- **Sub-rotinas** — mais de 30 procedimentos com `CALL` / `RET`
- **Pilha** — `PUSH` / `POP` para preservação de registradores entre chamadas
- **Acesso à memória** — endereçamento via `SI`, `DI`, `BX` com `BYTE PTR`
- **Aritmética de 8 bits** — `MUL`, `ADD`, `DEC`, `INC`, `XOR` para cálculo de índices
- **Segmentação** — modelo `.MODEL SMALL` com segmentos `.DATA`, `.STACK` e `.CODE`

---

## Dicas Estratégicas

- **Centro primeiro** — a posição (2,2) cobre 4 linhas de vitória possíveis
- **Cantos são fortes** — posições (1,1), (1,3), (3,1) e (3,3) cobrem diagonais
- **Bloqueie sempre** — se o adversário tiver 2 em linha, bloqueie imediatamente
- **Crie garfos** — tente montar duas ameaças simultâneas para forçar a vitória

---

## Referências

- ABEL, Peter. *IBM PC Assembly Language and Programming*. 5ª ed. Pearson, 2001.
- INTEL CORPORATION. *8086 Family User's Manual*. Intel Corporation, 1979.
- NORTON, Peter; SOCHA, John. *Peter Norton's Assembly Language Book for the IBM PC*. Brady Books, 1986.

---

## Autor

<div align="center">

Desenvolvido por **Rafael Sanguini Colagrossi**

<br>

&nbsp;&nbsp;&nbsp;
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/seu-perfil)
&nbsp;
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/seu-usuario)
&nbsp;
[![Portfolio](https://img.shields.io/badge/Portfolio-dc2626?style=flat&logo=vercel&logoColor=white)](#)
&nbsp;
[![Gmail](https://img.shields.io/badge/rafaelcolagrossi%40gmail.com-D14836?style=flat&logo=gmail&logoColor=white)](mailto:rafaelcolagrossi@gmail.com)

</div>

---


**Se este projeto te ajudou, deixa uma ⭐ no repositório — significa muito!**
