# IDS+: Structural & Semantic Encoding Protocol for LLMs

**IDS+** (Ideographic Description Sequences Plus) is a high-density, state-based encoding protocol designed to solve the "Token Waste" problem in CJK (Chinese, Japanese, Korean) character processing. 

Current LLMs often fail at rare CJK characters, falling back to 3-6 byte tokens (byte-fallback) and expanding the embedding matrix exponentially. IDS+ replaces these with a logical 1D grammar that provides **Etymological Awareness** to the model, drastically compressing the vocabulary and granting zero-shot generalization capabilities.

---

## ⚙️ 1. The Core Logic: State Toggling
IDS+ uses integers and specific symbols as "State Switches" to define the functional role of subsequent components.

### 1.1 Role Quantifiers
The protocol operates on three primary states:

| Quantifier | State | Description | Logic |
| :--- | :--- | :--- | :--- |
| **`n`** | **Semantic** | Meaning-bearing component | Activates Semantic mode for the next $n$ units. |
| **`+n`** | **Hybrid** | Semantic + Phonetic | Meaning and Sound combined (Default $n=1$ as `+`). |
| **(None)** | **Phonetic/Struct** | Sound or Carrier | Default state. Strictly structural or phonetic. |

### 1.2 Automatic Reset Rule
A quantifier's scope $n$ is strictly enforced. After $n$ character components are processed, the parser **automatically reverts** to the Default (Phonetic/Structural) state. This prevents "state amnesia" and ensures clean transitions.

---

## 🌊 2. The Quantum Leap: Stream-Based Parsing (Operator Immunity)
To maximize token economy and maintain Abstract Syntax Tree (AST) stability, IDS+ separates the **Spatial Layout** from the **Etymological Stream**.

* **Rule:** Quantifiers (`1`, `2`, `3`, etc.) count **ONLY** ideographic components (radicals). 
* **Operator Immunity:** Structural layout operators (`⿰`, `⿱`, `⿲`, `⿴`, etc.) are completely ignored by the quantifier count. 

This allows single quantifiers to govern components across complex spatial boundaries without breaking the tree.

---

## ⚡ 3. Advanced Compression: Directional Multipliers
For identical repetitive structures, IDS+ uses the hyphen `-` relative to the integer to define **Quantity** and **Role** simultaneously, generating the components on the fly.

| Syntax | Role | Action | Example |
| :--- | :--- | :--- | :--- |
| **`-n`** | **Semantic** | Repeat & assign Meaning | `⿰-1木` (2 Semantic Trees -> 林) |
| **`n-`** | **Phonetic** | Repeat & assign Sound | `⿰1-馬` (2 Phonetic Horses -> 騪) |

*(Note: The formula is always $1 + n$: The base unit + the $n$ repetitions).*

---

## 📝 4. Practical Breakdown & Examples

### A. Simple Semantic-Phonetic Compound
**Character:** **媽** (Mother)
**Logic:** `⿰1女馬`
* `1` -> Next 1 component is Semantic.
* `女` -> The "Female" radical (**Semantic** core).
* `馬` -> No prefix, reverts to default (**Phonetic** carrier).

### B. Multi-Component Enclosure (Demonstrating Operator Immunity)
**Character:** **閑** (Leisure/Gate)
**Logic:** `2⿵門木` *(Alternative inline: `⿵2門木`)*
* `2` -> Start Semantic mode (scope: 2 radicals).
* `⿵` -> Layout operator (Ignored by the counter).
* `門` -> First unit (**Semantic**).
* `木` -> Second unit (**Semantic**).

### C. AST Stability in Nested Trees
**Character:** **龘** (Flying Dragons)
**Logic:** `⿱3龍⿰龍龍`
Instead of using compression (`-n`) which would break standard prefix notation (AST) due to nested layouts, we use the stream logic:
* `⿱` -> Needs 2 arguments.
* `3` -> Next 3 radicals are Semantic.
* `龍` -> First argument for `⿱` (**Semantic**).
* `⿰` -> Second argument for `⿱`. Needs 2 arguments.
* `龍`, `龍` -> Arguments for `⿰` (Both **Semantic** as they fall under the `3` scope).

---

## 🏗️ 5. Technical Advantages
* **Vocabulary Compression:** Reduces LLM CJK vocabularies from ~50,000 characters to ~214 radicals + IDS operators.
* **Deterministic Parsing:** Zero ambiguity in AST construction or component roles.
* **Out-of-Distribution (OOD) Mastery:** LLMs can deduce the meaning of characters they have never seen during training by analyzing the IDS+ semantic tags.

## 📚 Related Documentation
For the formal mathematical grammar and nesting rules, see[SPECIFICATION.md](./SPECIFICATION.md).

---

## License
MIT License - Developed by Zurab Lomidze (oruc001)  
**Status:** Alpha v1.2 - Functional Specification with Stream-Based Logic Complete.
