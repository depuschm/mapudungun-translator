# 📖 Step-by-Step Guide: Mapudungun AI Translator

This guide explains every cell in the notebook in simple terms.

---

## Quick Start Checklist

Before running the notebook:
- [ ] Have a Google account (for Colab)
- [ ] Have a HuggingFace account (for saving your model)
- [ ] Get your HuggingFace token from: https://huggingface.co/settings/tokens

---

## Cell-by-Cell Explanation

### 📋 Step 1: Check GPU

```python
if torch.cuda.is_available():
    ...
```

**What it does:** Checks if you have a GPU available.

**Why it matters:** Training AI needs a GPU. Without it, training would take days instead of hours.

**If you see "No GPU found":**
1. Go to menu: `Runtime` → `Change runtime type`
2. Select `T4 GPU` from the dropdown
3. Click `Save`
4. Run the cell again

---

### 📦 Step 2: Install Libraries

```python
!pip install -q transformers peft datasets ...
```

**What it does:** Downloads and installs all the Python packages we need.

**The `!` symbol:** In Colab/Jupyter, `!` runs terminal commands (like in Command Prompt/Terminal).

**The `-q` flag:** Means "quiet" - less output text.

**Libraries explained:**
| Library | What it does |
|---------|-------------|
| `transformers` | HuggingFace's AI library - loads NLLB-200 |
| `peft` | Enables LoRA (efficient training) |
| `datasets` | Handles training data |
| `bitsandbytes` | Reduces memory usage (8-bit loading) |
| `beautifulsoup4` | Scrapes websites (Glosbe) |

---

### 🔑 Step 3: Login to HuggingFace

```python
from huggingface_hub import login
login(token=hf_token)
```

**What it does:** Connects to your HuggingFace account so you can save your model.

**Your token is like a password** - it proves you're you.

**Two ways to enter your token:**
1. **Colab Secrets (recommended):** Click 🔑 icon in left sidebar → Add secret named `HF_TOKEN`
2. **Manual:** Just paste it when asked

---

### 🌐 Step 4: Scrape Glosbe Data

```python
class GlosbeScraper:
    ...
```

**What it does:** 
1. Visits Glosbe.com pages
2. Finds Spanish-Mapudungun translation pairs
3. Saves them for training

**How web scraping works:**
```
Your Script → Requests page → Glosbe sends HTML → BeautifulSoup parses it → Extract translations
```

**Why we add delays (`time.sleep`):**
- Being polite to Glosbe's servers
- Avoiding getting blocked
- Scraping responsibly

**`MAX_WORDS = 500`:** Controls how much data to scrape. More = better model but slower.

---

### 🧹 Step 5: Clean Data

```python
def clean_text(text):
    ...
```

**What it does:** Removes bad/useless data.

**Things we remove:**
- Empty entries
- Duplicates
- Very long texts (errors)
- Texts that are just numbers
- Pairs where source = target

**Why cleaning matters:**
```
Bad data example:
  Spanish: "12345"
  Mapudungun: "12345"  ← Not useful!

Good data example:
  Spanish: "Buenos días"
  Mapudungun: "Ayin antü"  ← Useful!
```

**Data split (80/10/10):**
- **Training (80%):** Model learns from this
- **Validation (10%):** Checks progress during training
- **Test (10%):** Final quality check (model never sees this during training)

---

### 🤖 Step 6: Load NLLB-200

```python
model = AutoModelForSeq2SeqLM.from_pretrained(
    MODEL_NAME,
    quantization_config=quantization_config,
    ...
)
```

**What it does:** Downloads Meta's NLLB-200 translation model.

**8-bit quantization explained:**
```
Normal model:     Uses 32 bits per number → 2.4 GB memory
8-bit model:      Uses 8 bits per number  → 0.6 GB memory
                  ↓
              Fits in free GPU!
```

**Trade-off:** Slightly lower quality, but makes training possible for free.

---

### ⚡ Step 7: Configure LoRA

```python
lora_config = LoraConfig(
    r=16,
    lora_alpha=32,
    ...
)
```

**What is LoRA?**

Instead of training ALL 600 million parameters:
```
Traditional: Train 600,000,000 parameters ❌ (too expensive)
LoRA:        Train 3,000,000 parameters   ✅ (0.5% of total!)
```

**How LoRA works (simplified):**
```
Original model weights: [frozen, don't change]
         ↓
    + Small adapters: [these we train]
         ↓
    = Adapted model that knows Mapudungun
```

**Parameters explained:**
| Parameter | What it means |
|-----------|--------------|
| `r=16` | Size of adapters (higher = more learning capacity) |
| `lora_alpha=32` | How much adapters influence output |
| `lora_dropout=0.1` | Prevents overfitting (memorizing vs learning) |

---

### 📚 Step 8: Prepare Training Data

```python
def create_bidirectional_data(data):
    ...
```

**What it does:** Creates training examples for both directions.

**Bidirectional explained:**
```
Original pair:
  Spanish: "Hola" → Mapudungun: "Mari mari"

Becomes TWO training examples:
  1. "Hola" → "Mari mari"           (Spanish to Mapudungun)
  2. "Mari mari" → "Hola"           (Mapudungun to Spanish)
```

**Tokenization:** Converting text to numbers the model understands.
```
"Hola" → [1234, 5678, 2]  (token IDs)
```

---

### 🚀 Step 9: Train the Model

```python
trainer = Seq2SeqTrainer(...)
trainer.train()
```

**What happens during training:**

```
Epoch 1:  Model sees all data → Makes predictions → Calculates errors → Adjusts weights
Epoch 2:  Same data again → Better predictions → Smaller errors → More adjustments
Epoch 3:  Same data again → Even better → Even smaller errors → Fine adjustments
```

**Parameters explained:**

| Parameter | Meaning | Our value |
|-----------|---------|-----------|
| `num_train_epochs` | Times through all data | 3 |
| `batch_size` | Samples processed together | 4 |
| `learning_rate` | How big adjustments are | 0.0002 |
| `warmup_steps` | Gradual start period | 100 |

**What to watch:**
- `loss` should decrease over time
- If loss stops decreasing = model learned all it can

---

### 💾 Step 10: Save to HuggingFace

```python
api.upload_folder(...)
```

**What it does:** Uploads your trained model to HuggingFace Hub.

**Why save online?**
- Won't lose it when Colab disconnects
- Can use it from anywhere
- Can share with others
- Version control (HuggingFace tracks changes)

---

### 🧪 Step 11: Test Translations

```python
def translate(text, source_lang, target_lang, ...):
    ...
```

**How translation works:**

```
Input: "Buenos días"
    ↓
Tokenize: [1234, 5678, 9012]
    ↓
Model processes
    ↓
Output tokens: [4321, 8765]
    ↓
Decode: "Ayin antü"
```

**Beam search (`num_beams=5`):**
- Model considers 5 possible translations
- Picks the best one
- Better quality than just taking first guess

---

### 📊 Step 12: Evaluate with BLEU

```python
bleu = BLEU()
score = bleu.corpus_score(predictions, references)
```

**What is BLEU?**

BLEU (Bilingual Evaluation Understudy) measures translation quality by comparing with human translations.

**How it works:**
```
Reference: "The cat sat on the mat"
Prediction: "The cat sat on the mat"  → BLEU = 100 (perfect match)
Prediction: "A cat sat on the mat"    → BLEU = ~80 (close)
Prediction: "Dog runs in park"        → BLEU = ~5 (very different)
```

**Score interpretation:**
| BLEU Score | Quality Level |
|------------|---------------|
| 0-10 | Poor |
| 10-20 | Basic understanding |
| 20-30 | Good (for low-resource languages!) |
| 30-40 | High quality |
| 40+ | Excellent |

---

## Troubleshooting

### "CUDA out of memory"
**Solution:** Reduce `batch_size` from 4 to 2:
```python
per_device_train_batch_size=2
```

### "Rate limited" during scraping
**Solution:** The script handles this automatically (waits 30 seconds). Just be patient.

### Training loss not decreasing
**Solutions:**
1. More data (increase `MAX_WORDS`)
2. Lower learning rate (`learning_rate=1e-4`)
3. More epochs (`num_train_epochs=5`)

### Model outputs gibberish
**Possible causes:**
1. Not enough training data
2. Training didn't complete
3. Wrong language codes

---

## Glossary

| Term | Simple Explanation |
|------|-------------------|
| **Epoch** | One complete pass through all training data |
| **Batch** | Group of samples processed together |
| **Loss** | How wrong the model is (lower = better) |
| **Learning rate** | How big the adjustments are |
| **Tokenizer** | Converts text to numbers and back |
| **Fine-tuning** | Teaching existing model new things |
| **LoRA** | Efficient way to fine-tune (trains small adapters) |
| **BLEU** | Score measuring translation quality |
| **Quantization** | Reducing model size to fit in memory |

---

## Next Steps

After your first successful training:

1. **Get more data:** Increase `MAX_WORDS` to 1000+
2. **Train longer:** Change `num_train_epochs` to 5
3. **Find more sources:** Look for Mapudungun books, websites, etc.
4. **Join the community:** Connect with AmericasNLP or indigenous language projects

---

*Happy translating! 🌿*
