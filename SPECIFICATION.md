# IDS+ Protocol Specification v1.2

## 1. Introduction
IDS+ is a functional extension of Ideographic Description Sequences (IDS). It introduces a state-based quantifier system designed to optimize CJK character representation for Large Language Models (LLMs) by explicitly defining the semantic and phonetic roles of components, while preserving rigorous topological stability.

## 2. The State Machine Logic
The protocol operates as a finite state machine interacting with a prefix-notation Abstract Syntax Tree (AST). 

### 2.1 State Definitions
* **Phonetic/Structural (P) [Default]:** The component contributes primarily to the pronunciation or acts as a structural space-filler.
* **Semantic (S):** The component contributes primarily to the historical meaning/definition.
* **Hybrid (H):** The component contributes both semantically and phonetically.

### 2.2 Quantifiers & Scope
Quantifiers set the state for a specific number of subsequent **valid units** ($n$).

| Quantifier | Activated State | Scope ($n$) | Logic |
| :--- | :--- | :--- | :--- |
| **`n`** (e.g., 1, 2) | **Semantic** | Next $n$ units | Sets semantic mode for $n$ units. |
| **`+`** | **Hybrid** | Next 1 unit | Default hybrid marker ($n=1$). |
| **`+n`** (e.g., +2) | **Hybrid** | Next $n$ units | Sets hybrid mode for $n$ units. |
| **(None)** | **Phonetic/Struct**| Current unit | Reverts to default state. |

## 3. Syntax & Stream Rules

1. **Unit Definition:** A "unit" that decrements the quantifier scope $n$ is STRICTLY defined as a CJK character, radical, or valid stroke component. 
2. **Operator Immunity:** IDS layout operators (e.g., `⿰`, `⿱`, `⿲`, `⿴`) **DO NOT** count as units. The state machine pauses, allows the AST parser to register the operator, and resumes the scope evaluation on the next valid unit.
3. **Implicit Default:** Any component evaluated when the scope $n = 0$ is treated as Phonetic/Structural.
4. **Explicit Semantic:** A semantic component MUST fall under the scope of a preceding integer (e.g., `1`).
5. **AST Integrity Fallback:** In highly nested, repetitive structures (e.g., `⿱A⿰BC`), stream-based counting (`⿱3龍⿰龍龍`) MUST be prioritized over explicit directional compression (`-n`) to prevent AST destruction.
6. **Inline Placement:** Quantifiers can be placed inline directly before a unit, anywhere within the operator tree, allowing maximum flexibility (e.g., `⿱亡⿱口⿲月1貝凡`).

## 4. Evaluation Examples

**Sequence:** `⿰1氵+贏` (Character: 瀛)
* `⿰` -> Wait for 2 units.
* `1` -> Start Semantic mode ($n=1$).
* `氵` -> **Semantic** (Water). $n$ becomes $0$.
* `+` -> Start Hybrid mode ($n=1$).
* `贏` -> **Hybrid** (Win). $n$ becomes $0$.

**Sequence:** `⿱3龍⿰龍龍` (Character: 龘)
* `⿱` -> Wait for 2 units.
* `3` -> Start Semantic mode ($n=3$).
* `龍` -> **Semantic** ($n=2$). Fulfills arg 1 of `⿱`.
* `⿰` -> Wait for 2 units (Immune to $n$). Fulfills arg 2 of `⿱`.
* `龍` -> **Semantic** ($n=1$). Fulfills arg 1 of `⿰`.
* `龍` -> **Semantic** ($n=0$). Fulfills arg 2 of `⿰`. AST complete.
