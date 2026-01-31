# 🌿 Mapudungun AI Translator

A **Neural Machine Translation (NMT)** model for translating between **Spanish** and **Mapudungun** (both directions).

## What This Model Is (and Isn't)

| This Model IS | This Model is NOT |
|---------------|-------------------|
| Translation model (NMT) | Large Language Model (LLM) |
| Specialized for one task | General-purpose AI |
| Encoder-Decoder (Seq2Seq) | Decoder-only (like GPT) |
| 600M parameters | Billions of parameters |
| Fast and lightweight | Heavy and slow |

**Think Google Translate, not ChatGPT.** This model only translates text - it cannot chat, answer questions, or do other tasks.

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `Mapudungun_AI_Translator.ipynb` | Google Colab notebook to train the model |
| `STEP_BY_STEP_GUIDE.md` | Detailed explanation of each step |
| `MAPUDUNGUN_AI_RESEARCH.md` | Research on training approach |

---

## 🚀 Quick Start

1. Download `Mapudungun_AI_Translator.ipynb`
2. Open in [Google Colab](https://colab.research.google.com)
3. Enable GPU: Runtime → Change runtime type → T4 GPU
4. Run all cells (takes ~1-2 hours)
5. Your model will be saved to HuggingFace!

---

## Model Details

- **Model type:** Neural Machine Translation (NMT) / Seq2Seq
- **Base model:** facebook/nllb-200-distilled-600M
- **Fine-tuning method:** LoRA (Low-Rank Adaptation)
- **Training data:** Scraped from Glosbe.com
- **Languages:** Spanish (es) ↔ Mapudungun (arn)

### Language Code Note

Uses **Quechua (quy_Latn)** as a proxy code since Mapudungun is not in NLLB-200's original vocabulary. Mapudungun is a **language isolate** (no known relatives). Quechua was chosen because it shares **agglutinative morphology** - making it slightly more similar than Spanish structurally.

---

## About Mapudungun

Mapudungun is the language of the **Mapuche people** of Chile and Argentina.

| Feature | Description |
|---------|-------------|
| **Language family** | Isolate (no known relatives!) |
| **Morphology** | Agglutinative, polysynthetic |
| **Structure** | Heavy suffixing (20+ verbal slots) |
| **Alignment** | Unique hierarchical person-marking |
| **Speakers** | ~260,000 |

---

## Training Approach

### Why Fine-tuning (not training from scratch)?

| Approach | Data Needed | Cost | Time |
|----------|-------------|------|------|
| Train from scratch | 50M+ sentences | $50,000+ | Months |
| **Fine-tune NLLB-200** | 1,000+ sentences | **Free** | **Hours** |

### What is LoRA?

Instead of training all 600M parameters, LoRA trains small "adapter" layers (~0.5% of parameters). This makes training possible on free GPU tiers like Google Colab.

---

## Limitations

- **Only translates** - cannot chat, answer questions, or perform other tasks
- Works best with short sentences and common phrases
- May struggle with complex grammar or rare vocabulary
- Not suitable for official/legal translations
- Mapudungun's unique features may not be fully captured

---

## Purpose

This project was created to help **preserve and promote the Mapudungun language**.

Please use it respectfully and consider contributing back to the Mapuche community.

---

## Resources

- [Glosbe Spanish-Mapudungun Dictionary](https://glosbe.com/es/arn)
- [NLLB-200 Model](https://huggingface.co/facebook/nllb-200-distilled-600M)
- [Meta AI: No Language Left Behind](https://ai.meta.com/research/no-language-left-behind/)

---

*Pewkayal!* (Goodbye in Mapudungun) 🌿
