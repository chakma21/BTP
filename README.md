
# 📝 Stance Detection Project

This project focuses on **stance detection** by extracting **keywords/targets**, performing **sentiment analysis**, and mapping relationships between targets within articles. It combines statistical, semantic, and embedding-based NLP techniques to build a full pipeline from raw articles → sentiment-aware keyword graphs.

---

## 🚀 Workflow

### 1. **Keyword Classification**

* **Input**: Article file
* **Output**: `keywords/targets.csv`
* Extracts keywords using **RAKE**, **YAKE**, and **KeyBERT**.
* Preprocessing: cleaned sentences → concatenated into a single paragraph for better context.
* Threshold-based keyword selection:

  * RAKE & KeyBERT → keep high-score keywords.
  * YAKE → keep low-score keywords (higher relevance).
* Results compiled into a CSV file.

| Feature / Method | RAKE                                    | YAKE                                | KeyBERT                    |
| ---------------- | --------------------------------------- | ----------------------------------- | -------------------------- |
| **Type**         | Statistical (frequency & co-occurrence) | Statistical (multi-feature scoring) | Semantic (embedding-based) |
| **Strengths**    | Fast, simple                            | Language-independent, adaptable     | Captures meaning & nuance  |
| **Limitations**  | Ignores semantics                       | Sensitive to weights                | Slower, needs embeddings   |

---

### 2. **Sentence Extraction**

* **Input**: Article file
* **Output**: Sentences list
* Uses **NLTK Punkt tokenizer**.
* Handles punctuation and avoids splitting inside abbreviations.
* Cleans leading/trailing whitespace.

---

### 3. **Sentiment Analysis**

* **Input**: Sentences list
* **Output**: `sentences + sentiment.csv`
* Based on **ELECTRA architecture**:

  1. Pre-trained fine-tuned model (`jbeno/electra-base-classifier-sentiment`)
  2. Fine-tuned Google ELECTRA model (`google/electra-base-discriminator`)
* Current experiments use the **jbeno model** applied to the demonetization dataset.

---

### 4. **Finding Relations Between Targets**

* **Input**: Sentences+sentiment file + targets list file
* **Output**: `keywords/targets + sentiments.csv`
* Matches keywords in sentences via regex.
* If multiple keywords co-occur in a sentence → relation is formed.
* Relation inherits sentence sentiment.

---

### 5. **Target Generalization**

* **Input**: `keywords/targets.csv`
* **Output**: `clustered keywords.csv`
* Uses **SentenceTransformer (`all-MiniLM-L6-v2`)** to generate embeddings.
* Applies **hierarchical clustering** with cosine distance.
* Threshold parameter controls granularity (strict vs. loose clustering).

---

### 6. **Keyword Generalization Mapping**

* **Input**: Clustered keywords map + targets+sentiment file
* **Output**: Unified file with keywords → generalized form + sentiment
* Ensures consistent grouping of keywords (e.g., *AI* → *Artificial Intelligence*).

---

### 7. **Keyword Sentiment Graph**

* **Input**: Final CSV (generalized keywords + sentiments)
* **Output**: Sentiment graph
* Graph shows keyword connections with sentiment-aware edges:

  * ✅ Green → Positive
  * ❌ Red → Negative
  * ⚪ Gray → Neutral
* Nodes = keywords, edges = co-occurrence + sentiment context.

---

## 📊 Example Datasets

* **Demonetization dataset** (with/without sampling)
* **Ball tampering dataset**
* **Catalan**
* **Harvey**

---

## 🛠️ Tech Stack

* **Python** (NLP pipeline)
* **NLTK** (sentence tokenizer)
* **RAKE, YAKE, KeyBERT** (keyword extraction)
* **Hugging Face Transformers** (ELECTRA models)
* **SentenceTransformers** (embeddings & clustering)
* **Matplotlib / NetworkX** (graphs & visualization)

---

## 📌 Future Improvements

* Fine-tune ELECTRA on stance datasets
* Experiment with contextual target detection (beyond regex)
* Improve clustering thresholds dynamically
* Interactive graph visualization

---

## 👩‍💻 Contributors

* Shiny Chakma
* Priya Keshri
* Toko Saniya

