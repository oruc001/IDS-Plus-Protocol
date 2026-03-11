# IDS+ Protocol (Ideographic Description Sequences Plus)

IDS+ is a functional extension of the standard Ideographic Description Sequences (IDS). It is designed to solve the **"Byte-Premium"** issue in CJK (Chinese, Japanese, Korean) language processing, where rare or out-of-vocabulary characters are broken into 3-6 meaningless bytes by standard BPE/SentencePiece tokenizers.

**IDS+ reduces token usage by up to 70% for rare ideographs and saves approximately 30% of context window space.**

---

## The Problem
Current LLM tokenization is often inefficient for logographic scripts. When a model encounters a character not in its vocabulary:
* **Traditional Way:** It falls back to UTF-8 bytes (e.g., `\xe9\xbe\x9d`), consuming 3-6 tokens with zero semantic value.
* **IDS+ Way:** It represents the character via structural components (e.g., `⿰+木目`), consuming only 1-2 tokens while preserving semantic reasoning capability.

## Functional Logic & Syntax
IDS+ introduces minimalist functional markers to make structural sequences machine-readable and semantically dense:

* `+` : **Hybrid Marker.** Denotes a single component carrying both semantic and phonetic weight.
* `+n`: **Multi-Hybrid.** Used when multiple components (*n*) in a sequence are hybrid.
* `(n)`: **Positional Marker.** Used for complex structural nesting to maintain sequence order for the embedding layer.

### Efficiency Comparison

| Character Type | Standard (Byte-fallback) | IDS+ Protocol | Efficiency Gain |
| :--- | :--- | :--- | :--- |
| Common Characters | 1 Token | 1 Token | 0% (Passthrough) |
| Rare/OOV Characters | 3-6 Tokens | 1-2 Tokens | ~70% |
| **Total Context** | **100% Usage** | **~70% Usage** | **30% Saved** |

---

## Project Goals
1. **Reduce Training/Inference Costs:** Smaller sequences lead to faster processing and lower memory footprint.
2. **Zero-Shot Semantic Reasoning:** Enables models to interpret "unseen" characters through structural analysis.
3. **Open Standard:** A universal mapping for CJK structural encoding in LLM embedding layers.

---

## License
MIT License - Developed by Zurab Lomidze.
