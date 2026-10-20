# 🎮 Jogo da Velha — Assembly 8086

> Uma implementação completa do Jogo da Velha em Assembly 8086, com modos Jogador vs. Jogador e Jogador vs. Computador (IA).

<p align="center">
  <img src="https://img.shields.io/badge/Assembly-8086-6E4C13?style=flat-square&logo=intel&logoColor=white" alt="Assembly 8086"/>
  <img src="https://img.shields.io/badge/License-MIT-22c55e?style=flat-square" alt="Licença MIT"/>
  <img src="https://img.shields.io/badge/Platform-MS--DOS%20%7C%20DOSBox-blue?style=flat-square" alt="Plataforma"/>
  <img src="https://img.shields.io/badge/Status-Conclu%C3%ADdo-success?style=flat-square" alt="Status"/>
</p>

---

## 📖 Visão geral

Este projeto é um jogo da velha totalmente funcional, desenvolvido em Assembly 8086 puro e executado em modo texto no MS-DOS ou no emulador DOSBox. O jogo possui dois modos: **Jogador vs. Jogador** e **Jogador vs. Computador**, com um oponente controlado por uma inteligência artificial baseada em regras.

A implementação explora conceitos de programação de baixo nível, incluindo manipulação direta de memória, operações com registradores, gerenciamento de pilha e interrupções DOS/BIOS, sem utilizar abstrações de linguagens de alto nível.

---

## ✨ Funcionalidades

- **Dois modos de jogo** — PvP (jogador contra jogador) e PvC (jogador contra inteligência artificial).
- **IA baseada em regras** — o computador segue uma estratégia de prioridades: vencer → bloquear → ocupar o centro → primeira posição disponível.
- **Validação de entrada** — rejeita posições inválidas e células já ocupadas.
- **Sistema de revanche** — permite iniciar outra partida sem reiniciar o programa.
- **Mais de 30 procedimentos** — código modular organizado com `CALL`/`RET` e preservação de registradores na pilha.
- **980 linhas de código** — implementação em Assembly 8086 puro, sem bibliotecas externas.

---

## 🎯 Como jogar

### Menu

```text
JOGO DA VELHA - 8086
Escolha o modo de jogo:
1 - Jogador vs Jogador
2 - Jogador vs Computador
Opcao: _
```

### Tabuleiro

```text
   1   2   3
1    |   |
  -----------
2    |   |
  -----------
3    |   |
```

Informe a **linha** (1–3) e a **coluna** (1–3) para posicionar sua marca no tabuleiro. O jogador X sempre começa.

---

### Passo a passo completo

#### 1. Iniciando o jogo

Execute o programa e escolha um dos modos disponíveis:

- **Opção 1 (PvP):** dois jogadores humanos alternam suas jogadas. O jogador X começa, seguido pelo jogador O.
- **Opção 2 (PvC):** você joga com X contra o computador, que utiliza O. Você sempre realiza a primeira jogada.

#### 2. Realizando uma jogada

Durante seu turno, o jogo solicita uma linha e uma coluna:

```text
Vez do Jogador X
Digite a linha (1-3): 2
Digite a coluna (1-3): 2
```

O jogo valida as informações inseridas:

- **Posição inválida:** valor fora do intervalo de 1 a 3. Uma mensagem de erro será exibida e você poderá tentar novamente.
- **Célula ocupada:** a posição já contém uma marca. Será necessário escolher outra.
- **Jogada válida:** sua marca será posicionada e o tabuleiro será atualizado na tela.

Após uma jogada válida, o turno passa para o outro jogador ou para o computador.

#### 3. Vencendo a partida

O primeiro jogador a alinhar **três marcas iguais** em uma linha vence. As combinações vencedoras são:

- **3 linhas horizontais** — por exemplo, a primeira linha, com as células 1, 2 e 3.
- **3 colunas verticais** — por exemplo, a primeira coluna, com as células 1, 4 e 7.
- **2 diagonais** — células 1, 5 e 9 ou células 3, 5 e 7.

Quando um jogador vence, o programa exibe uma mensagem de vitória:

```text
----JOGADOR X VENCEU!----
```

#### 4. Empate

Se as nove células estiverem preenchidas e nenhum jogador tiver alinhado três marcas, a partida termina empatada:

```text
----EMPATE!----
```

#### 5. Jogar novamente

Após uma vitória ou empate, o jogo pergunta se você deseja iniciar outra partida:

```text
Deseja jogar novamente? (S/N): _
```

Digite `S` para começar uma nova partida ou `N` para encerrar o programa.

---

### 💡 Dicas e estratégias

#### Jogador vs. Jogador (PvP)

| Dica | Explicação |
|---|---|
| **Priorize o centro** | A célula central (2,2) participa de quatro combinações vencedoras: duas diagonais, uma linha e uma coluna. Controlá-la amplia suas possibilidades. |
| **Fique atento aos cantos** | Os cantos (1,1), (1,3), (3,1) e (3,3) participam de três combinações vencedoras cada. Se o centro estiver ocupado, considere um canto. |
| **Bloqueie o adversário** | Se o oponente tiver duas marcas alinhadas, ocupe imediatamente a terceira posição para impedir a vitória. |
| **Crie bifurcações** | Uma bifurcação ocorre quando você cria duas ameaças de vitória simultâneas. O adversário não consegue bloquear ambas com apenas uma jogada. |
| **Evite começar pelas bordas** | As células laterais (1,2), (2,1), (2,3) e (3,2) participam de apenas duas combinações vencedoras cada, sendo menos vantajosas como primeira jogada. |

#### Jogador vs. Computador (PvC)

A IA segue uma prioridade fixa: **vencer → bloquear → centro → primeira posição disponível**. Veja como utilizar essa estratégia a seu favor:

| Dica | Explicação |
|---|---|
| **Você começa: aproveite isso** | Como você sempre joga com X e começa a partida, ocupar o centro é uma boa estratégia inicial. |
| **Tente criar uma bifurcação** | Comece por um canto, como (1,1). Se a IA ocupar o centro, considere o canto oposto (3,3). A possibilidade de criar duas ameaças dependerá das demais jogadas. |
| **A IA aproveita oportunidades de vitória** | Se o computador tiver duas marcas em uma linha e a terceira posição estiver livre, ele tentará completar a sequência. |
| **A IA bloqueia ameaças imediatas** | Se você tiver duas marcas alinhadas, o computador tentará bloquear a terceira posição. Antecipe essa reação ao planejar suas jogadas. |
| **O empate é o melhor resultado contra uma IA perfeita** | Com ambos os lados jogando perfeitamente, o jogo da velha termina empatado. Para evitar a derrota, não deixe oportunidades de vitória para o computador. |
| **Explore os cantos** | Começar por um canto pode criar oportunidades estratégicas, dependendo da resposta do computador. A possibilidade de vitória depende da sequência de jogadas. |

---

## 🤖 Estratégia da inteligência artificial

O oponente controlado pelo computador (jogador O) utiliza um algoritmo determinístico baseado em prioridades:

| Prioridade | Ação | Descrição |
|---|---|---|
| 1 | **Vencer** | Completa uma linha quando já possui duas marcas alinhadas e a terceira posição está vazia. |
| 2 | **Bloquear** | Impede que o jogador humano vença na próxima jogada. |
| 3 | **Centro** | Ocupa a célula central (índice 4), caso esteja disponível. |
| 4 | **Alternativa** | Escolhe a primeira célula vazia encontrada no tabuleiro. |

Essa estratégia permite que a IA aproveite oportunidades de vitória e bloqueie ameaças imediatas. Entretanto, ela não garante uma jogada perfeita em todas as situações, pois não avalia todas as sequências futuras possíveis.

---

## 🏗️ Arquitetura

### Modelo de memória

```asm
.MODEL SMALL
.STACK 100h
```

### Segmento de dados

```asm
tabuleiro           DB 9 DUP(' ')   ; Tabuleiro 3x3 representado por um vetor linear
modo_jogo           DB 0            ; 1 = PvP, 2 = PvC
jogador_atual       DB 'X'          ; Símbolo do jogador atual
jogadas_realizadas  DB 0            ; Contador de jogadas (empate na 9ª)
```

### Conversão de coordenadas para índice

```asm
; indice = (linha - 1) × 3 + (coluna - 1)
DEC BL       ; linha - 1
DEC BH       ; coluna - 1
MOV AL, BL
MOV CL, 3
MUL CL       ; AL = (linha - 1) × 3
ADD AL, BH   ; AL = índice final
```

### Principais procedimentos

| Procedimento | Responsabilidade |
|---|---|
| `MAIN` | Loop principal do jogo, menu e detecção de vitória ou empate. |
| `INICIALIZAR_JOGO` | Reinicializa o tabuleiro e as variáveis de controle. |
| `MOSTRAR_TABULEIRO` | Exibe a grade 3×3 na tela. |
| `REALIZAR_JOGADA_HUMANO` | Lê, valida e processa a jogada humana. |
| `REALIZAR_JOGADA_COMPUTADOR` | Executa a lógica da IA para as jogadas do computador. |
| `TENTAR_COMPLETAR_LINHA` | Detecta padrões com duas marcas e uma posição vazia. |
| `VERIFICAR_VITORIA` | Verifica as oito combinações vencedoras. |
| `TROCAR_JOGADOR` | Alterna entre X e O. |
| `LIMPAR_TELA` | Limpa a tela utilizando a interrupção `INT 10h`. |
| `IMPRIMIR_STRING` | Exibe strings terminadas em `$` utilizando `INT 21h/09h`. |
| `LER_CARACTERE` | Lê um caractere com eco utilizando `INT 21h/01h`. |

---

## 🔌 Interrupções utilizadas

| Interrupção | Função | Utilização |
|---|---|---|
| `INT 21h / 01h` | Lê um caractere com eco. | Entrada do jogador. |
| `INT 21h / 02h` | Exibe um caractere. | Renderização do tabuleiro. |
| `INT 21h / 09h` | Exibe uma string terminada em `$`. | Mensagens da interface. |
| `INT 21h / 08h` | Lê um caractere sem eco. | Pausa entre turnos. |
| `INT 21h / 4Ch` | Encerra o programa. | Saída da aplicação. |
| `INT 10h / 06h` | Executa rolagem da tela para cima. | Limpeza da tela. |
| `INT 10h / 02h` | Define a posição do cursor. | Reposicionamento após limpar a tela. |

---

## 📂 Estrutura do projeto

```text
jogo-da-velha-assembly/
├── src/
│   └── jogo_da_velha.asm    # Código-fonte (980 linhas)
├── .gitignore
├── LICENSE
└── README.md
```

---

## ⚙️ Compilação

### Requisitos

- **MASM** ou **TASM** — montador Assembly.
- Ambiente **MS-DOS** ou emulador **DOSBox**.

### Utilizando MASM

```bat
cd src
MASM jogo_da_velha.asm;
LINK jogo_da_velha.obj;
```

### Utilizando TASM

```bat
cd src
TASM jogo_da_velha.asm
TLINK jogo_da_velha.obj
```

### Alternativa: VS Code + extensão ASM

1. Instale a extensão [ASM Code Lens](https://marketplace.visualstudio.com/items?itemName=maziac.asm-code-lens).
2. Configure o MASM ou TASM no seu ambiente.
3. Abra o arquivo `src/jogo_da_velha.asm`.
4. Clique com o botão direito e selecione **Run ASM Code**.

---

## ▶️ Execução

### MS-DOS

```bat
jogo_da_velha.exe
```

### DOSBox (Windows moderno)

```text
mount c c:\path\to\project
c:
cd src
jogo_da_velha.exe
```

> **Requisitos mínimos:** MS-DOS ou emulador compatível, processador Intel 8086 ou superior e 64 KB de RAM.

---

## 🧠 Conceitos aplicados

- **Saltos condicionais** — utilização de `JE`, `JNE`, `JL`, `JG`, `JB` e `JA` para controle de fluxo.
- **Sub-rotinas** — mais de 30 procedimentos organizados com `CALL` e `RET`.
- **Operações com a pilha** — utilização de `PUSH` e `POP` para preservação de registradores.
- **Endereçamento de memória** — utilização de `SI`, `DI`, `BX` e `BYTE PTR`.
- **Aritmética de 8 bits** — operações `MUL`, `ADD`, `DEC`, `INC` e `XOR` para cálculos e manipulação de índices.
- **Segmentação de memória** — utilização de `.MODEL SMALL` com os segmentos `.DATA`, `.STACK` e `.CODE`.

---

## 📚 Referências

- ABEL, Peter. *IBM PC Assembly Language and Programming*. 5ª ed. Pearson, 2001.
- Intel Corporation. *8086 Family User's Manual*. 1979.
- NORTON, Peter; SOCHA, John. *Peter Norton's Assembly Language Book for the IBM PC*. Brady Books, 1986.

---

## 👤 Autor

**Rafael Sanguini Colagrossi** <br> 
--  Desenvolvedor de software apaixonado por Códigos, Inteligência Artificial, Tecnologia e Desenvolvimento Full-Stack.

Criando softwares modernos com foco em desempenho, arquitetura limpa e excelente experiência do usuário. --

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://linkedin.com/in/rafaelsanguini)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/rsanguini)
[![Portfólio](https://img.shields.io/badge/Portf%C3%B3lio-000000?style=flat-square&logo=vercel&logoColor=white)](https://rafaelsanguini.vercel.app)
[![E-mail](https://img.shields.io/badge/E--mail-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:rafaelcolagrossi@gmail.com)

---

⭐ Se este projeto foi útil para você, considere deixar uma estrela no repositório!
