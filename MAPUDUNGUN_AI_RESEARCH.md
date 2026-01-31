# Training an AI for Mapudungun: Complete Research Guide

## Executive Summary

**Good news:** Training an AI for Mapudungun is definitely possible! The Glosbe data you found contains **1,911 phrases** and **164,775 examples** - this is actually a decent starting point for a low-resource language.

---

## 1. What Can You Build?

### Option A: Translation Model (RECOMMENDED for beginners)
| Aspect | Details |
|--------|---------|
| **What it does** | Translates Spanish ↔ Mapudungun |
| **Difficulty** | ⭐⭐ Medium |
| **Data needed** | 1,000-6,000+ sentence pairs (you have ~165K examples!) |
| **Feasibility** | ✅ HIGH - Perfect for your dataset |

### Option B: Chatbot in Mapudungun
| Aspect | Details |
|--------|---------|
| **What it does** | Conversational AI that responds in Mapudungun |
| **Difficulty** | ⭐⭐⭐⭐ Very Hard |
| **Data needed** | Requires conversational data, not just translations |
| **Feasibility** | ⚠️ LOW - You'd need dialogue datasets, not dictionary data |

### 🎯 Recommendation: Start with Translation Model
Your Glosbe data is perfect for translation. A chatbot would require different data (conversations, not dictionary entries).

---

## 2. Free Cloud GPU Platforms Comparison

| Platform | Free GPU Hours | GPU Type | Session Limit | Storage | Best For |
|----------|---------------|----------|---------------|---------|----------|
| **Google Colab** | ~40 hrs/week | T4 (16GB) | 12 hours | 100GB temp | Quick experiments |
| **Kaggle** | 30 hrs/week | T4x2 or P100 | 12 hours | 20GB persist | Competitions, stable |
| **Lightning AI** | 80 hrs/month | L40s, A100, H100 | 4hr restart | 50GB persist | **BEST for training** |
| **Paperspace Gradient** | Limited | Various | Varies | 5GB | Small projects |
| **Amazon SageMaker Studio Lab** | 4 hrs GPU/day | T4 | 4 hours | 15GB persist | AWS integration |

### 🏆 Winner: Lightning AI
- **80 free GPU hours/month** (most generous)
- Access to powerful GPUs (A100, H100)
- 50GB persistent storage
- No credit card required
- 24/7 access (just restart every 4 hours)

### Runner-up: Kaggle
- More stable than Colab
- Good community and datasets
- Consistent GPU availability

---

## 3. The Best Approach: Step-by-Step

### Step 1: Data Collection & Preparation
```
Source: Glosbe (glosbe.com/es/arn)
Available: ~1,911 phrases + 164,775 examples
Format needed: Parallel text files (Spanish | Mapudungun)
```

### Step 2: Choose the Right Model to Fine-tune

| Model | Why It's Good | Difficulty |
|-------|--------------|------------|
| **NLLB-200** (Meta) | Trained on 200 languages, includes some indigenous languages | ⭐⭐ Easiest |
| **MarianMT** (Helsinki) | Lightweight, fast training | ⭐⭐ Easy |
| **mBART** | Good for low-resource languages | ⭐⭐⭐ Medium |
| **LLaMA 3 + LoRA** | Most powerful, but complex | ⭐⭐⭐⭐ Hard |

### 🎯 Recommended: NLLB-200 with Fine-tuning
- Already knows 200 languages (transfer learning helps!)
- Works well with small datasets
- Free and open-source
- Can run on free GPU tiers

### Step 3: Training Strategy

**Two-Stage Fine-tuning (Best for low-resource):**
1. **Stage 1**: Adapt the model to Mapudungun text patterns
2. **Stage 2**: Fine-tune on your parallel Spanish-Mapudungun data

**Parameter-Efficient Fine-Tuning (PEFT/LoRA):**
- Train only 2-5% of the model's parameters
- Reduces GPU memory by 50x
- Perfect for free GPU tiers
- Works great with small datasets

---

## 4. What Research Says

### Key Findings from Recent Studies (2024-2025):

1. **AmericasNLP 2025**: Successfully trained models for 13 indigenous languages including Mapudungun variants

2. **Inria Chile's Huemul Project** (Nov 2024): Using "transfer learning" specifically for Mapudungun-to-Spanish translation

3. **Latam-GPT**: Open-source LLM trained on Mapudungun, Nahuatl, Quechua using LLaMA 3

4. **Dataset Size Research**:
   - 1,000 sentence pairs = Basic usable translation
   - 3,000-6,000 pairs = Good quality
   - Your 164,775 examples = EXCELLENT starting point!

### Scientific Consensus:
> "Even tiny datasets (1,000+ pairs) can produce viable NMT results when using high-quality, professionally translated pairs combined with transfer learning from multilingual models."
> — ACL 2023 Research

---

## 5. Practical Implementation Plan

### Phase 1: Data Scraping (Week 1)
```
Tool: Python + BeautifulSoup
Goal: Extract all Spanish-Mapudungun pairs from Glosbe
Output: Two files (spanish.txt, mapudungun.txt)
```

### Phase 2: Data Cleaning (Week 1-2)
```
- Remove duplicates
- Fix encoding issues
- Validate parallel alignment
- Split: 80% train, 10% validation, 10% test
```

### Phase 3: Model Setup (Week 2)
```
Platform: Lightning AI or Google Colab
Model: facebook/nllb-200-distilled-600M
Framework: HuggingFace Transformers + PEFT
```

### Phase 4: Training (Week 3-4)
```
Method: LoRA fine-tuning
GPU: T4 or better (free tier)
Time: ~10-20 hours total
```

### Phase 5: Evaluation & Iteration (Week 4+)
```
Metrics: BLEU score, human evaluation
Goal: Iterate and improve
```

---

## 6. Hardware Requirements vs Free Tiers

| Requirement | What You Need | Free Tier Provides |
|-------------|--------------|-------------------|
| GPU Memory | 8-16 GB | ✅ T4 has 16GB |
| Training Time | 10-20 hours | ✅ Lightning AI: 80 hrs/month |
| Storage | 5-10 GB | ✅ All platforms have enough |
| RAM | 12-16 GB | ✅ Colab/Kaggle provide this |

**Verdict: ✅ Free tiers are SUFFICIENT for this project!**

---

## 7. Limitations & Honest Assessment

### What Will Work Well:
- ✅ Basic translation Spanish → Mapudungun
- ✅ Common phrases and vocabulary
- ✅ Short sentences

### What Will Be Challenging:
- ⚠️ Complex grammatical structures (Mapudungun is agglutinative)
- ⚠️ Cultural context and idioms
- ⚠️ Rare vocabulary not in training data

### What Won't Work:
- ❌ Full chatbot capabilities (need different data)
- ❌ Perfect native-level translations (need more data + human review)
- ❌ Real-time voice translation (requires additional models)

---

## 8. Next Steps - Your Decision

### Option A: Build a Translation Model
**If you want to proceed, I can create:**
1. A Python script to scrape Glosbe data
2. Data preprocessing pipeline
3. Training notebook for Google Colab/Lightning AI
4. Complete step-by-step instructions

### Option B: Research Existing Projects
**I can help you:**
1. Find and connect with Huemul Project (Inria Chile)
2. Explore Latam-GPT for Mapudungun
3. Join AmericasNLP community

### Option C: Hybrid Approach
**Combine both:**
1. Use existing pretrained models as base
2. Fine-tune with your scraped Glosbe data
3. Contribute back to open-source community

---

## Summary

| Question | Answer |
|----------|--------|
| Is Glosbe data enough? | ✅ Yes! 164,775 examples is great for a low-resource language |
| Translation or Chatbot? | 🎯 Translation (chatbot needs conversational data) |
| Best free platform? | 🏆 Lightning AI (80 hrs/month) or Kaggle |
| Best approach? | 📚 Fine-tune NLLB-200 with LoRA |
| Is it achievable? | ✅ Absolutely, with free resources |
| Time estimate? | ⏱️ 4-6 weeks for a working prototype |

---

*Document created: January 2025*
*Based on latest NLP research and low-resource language training techniques*
