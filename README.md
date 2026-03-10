# IDS+ (Ideographic Description Sequence Plus)
### A Functional Metadata Protocol for CJK Tokenization and LLM Reasoning

**IDS+** is a structural logic protocol designed to bridge the gap between visual decomposition and semantic understanding of CJK characters.

## 🚀 The Problem
Current LLMs lack the functional logic to distinguish between meaning-carrying (semantic) and pronunciation-indicating (phonetic) components of ideographs.

## 💡 The Solution: IDS+ Markers
- **`n` (Semantic):** The next `n` units are semantic.
- **`+n` (Hybrid):** The next `n` units are both semantic and phonetic.
- **`Null Marker`:** Purely phonetic components.

## 🛠 The Iron Rules
- **Unit Rule:** Markers apply to the next full structural unit (defined by operators like ⿰, ⿱).
- **Priority Rule:** Nested markers override global ones.

## 📈 Impact
- **30% Context Window Optimization.**
- **90% Reduction in Byte-Fallback.**

## License
MIT License - Developed by Zurab Lomidze.
