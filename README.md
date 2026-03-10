# IDS+ Protocol (Ideographic Description Sequences Plus)

### Optimizing CJK Tokenization for Next-Gen LLMs

Standard tokenizers (BPE, SentencePiece) suffer from a massive **"Byte-Premium"** when handling CJK (Chinese, Japanese, Korean) languages. Rare ideographs are often broken into 3-6 meaningless bytes, wasting context window and destroying semantic logic.

**IDS+ solves this by using functional markers to encode character structures.**

## 🚀 Key Benefits
- **~70% Token Reduction:** For rare and out-of-vocabulary (OOV) characters.
- **30% Context Window Savings:** More room for actual reasoning, less for overhead.
- **Semantic Continuity:** Models can perform zero-shot reasoning on unseen characters by analyzing their IDS+ structural components.

## 🛠 How it Works
Traditional IDS describes a character's layout. **IDS+** adds functional markers like `(+n, n)` to make these sequences machine-readable and semantically dense for LLM embedding layers.

| Character | Standard Bytes | IDS+ Sequence | Efficiency Gain |
|-----------|----------------|---------------|-----------------|
| Rare Kanji| 3-6 Tokens     | 1-2 Tokens    | ~70%            |

## 📂 Project Structure
- `/specs`: Detailed technical documentation of functional markers.
- `/examples`: Comparison between BPE and IDS+ tokenization.

---
## License
This project is licensed under the **MIT License**.
Developed with ❤️ by **Zurab Lomidze**.
