# IDS+ Protocol Specification v1.1

## 1. Introduction
IDS+ is a functional extension of Ideographic Description Sequences (IDS). It introduces a state-based quantifier system designed to optimize CJK character representation for Large Language Models (LLMs) by explicitly defining the semantic and phonetic roles of components.

## 2. The State Machine Logic
The protocol operates as a finite state machine. The functional "role" of any given component is determined by the preceding quantifier or the system's default state.

### 2.1 State Definitions
* **Phonetic (P) [Default]:** The component contributes primarily to the pronunciation or acts as a structural carrier.
* **Semantic (S):** The component contributes primarily to the meaning/definition.
* **Hybrid (H):** The component contributes both semantically and phonetically.

### 2.2 Quantifiers & Scope
Quantifiers act as "toggles" that set the state for a specific number of subsequent components ($n$).

| Quantifier | Activated State | Scope ($n$) | Logic |
| :--- | :--- | :--- | :--- |
| **`n`** (e.g., 1, 2) | **Semantic** | Next $n$ units | Sets semantic mode for $n$ components. |
| **`+`** | **Hybrid** | Next 1 unit | Default hybrid marker ($n=1$). |
| **`+n`** (e.g., +2) | **Hybrid** | Next $n$ units | Sets hybrid mode for $n$ components. |
| **(None)** | **Phonetic** | Current unit | Reverts to default state. |



## 3. Syntax Rules
1. **Implicit Phonetic:** Any component without a preceding quantifier is treated as Phonetic.
2. **Explicit Semantic:** A semantic component MUST be preceded by at least `1`.
3. **Automatic Reversion:** Once the scope $n$ of a quantifier is exhausted, the system immediately reverts to the Default (Phonetic) state.
4. **Nesting:** Parentheses `()` are used to group complex sub-structures, which are then treated as a single unit within the quantifier's scope.

## 4. Logical Example
**Sequence:** `⿰1氵+贏` (Character: 瀛)
* `1` -> Start Semantic mode (scope: 1).
* `氵` -> **Semantic** (Water).
* `+` -> Start Hybrid mode (scope: 1).
* `贏` -> **Hybrid** (Win).

**Sequence:** `2XXP2XX` (Complex 6-unit structure)
* `2` -> First 2 units are **Semantic**.
* (None) -> Next 2 units are **Phonetic** (Default).
* `2` -> Last 2 units are **Semantic**.
