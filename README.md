
---

# Automated Joke Generation System

This project generates jokes in the form:  
**"I like my X like I like my Y, Z₁ and Z₂."**  
where **X** is a user-provided noun, **Y** is automatically selected from a set of common nouns (based on frequency from the subtle dataset), and **Z₁** and **Z₂** are adjectives (selected from the GloVe vocabulary using cosine similarity).

## Prerequisites

- **Python 3.7 or higher**
- **pip** (Python package installer)

## Environment Setup

We recommend using a virtual environment. You can create one using `venv` (or any tool of your choice, e.g., Conda).

### Using `venv`:

1. **Create a virtual environment:**

   ```bash
   python -m venv joke_env
   ```

2. **Activate the environment:**

   - On Windows:
     ```bash
     joke_env\Scripts\activate
     ```
   - On macOS/Linux:
     ```bash
     source joke_env/bin/activate
     ```

3. **Upgrade pip (optional but recommended):**

   ```bash
   pip install --upgrade pip
   ```

## Installing Dependencies

Install the required packages using `pip`:

```bash
pip install pandas spacy numpy tqdm gensim
```

Then, download the English model for spaCy:

```bash
python -m spacy download en_core_web_sm
```

## Preparing Data and Models

1. **GloVe Vectors:**  
   - You need a pre-saved native-format GloVe file named `word_vectors.kv`.  
   - If you haven't converted your raw GloVe file yet, use the provided conversion script (e.g., `convert_glove.py`) to convert `glove.840B.300d.txt` into Gensim's native format and save it as `word_vectors.kv`.

2. **Subtle Dataset:**  
   - Place your subtle dataset CSV file (with columns `Word`, `POS`, and `FREQcount`) in the project directory, named `subtle_with_pos.csv`.

3. **Caching:**  
   - The script caches two files:  
     - `cached_glove_vocab.pkl` for the filtered GloVe vocabulary (nouns and adjectives).  
     - `cached_subtle_vocab.pkl` for the subtle dataset details.
   - These will be automatically created on the first run.

## Running the Script

The main script for generating jokes is called (for example) `generate_jokes_console.py`. To run it, simply execute:

```bash
python generate_jokes_console.py
```

You will be prompted to enter a noun (e.g., "man", "dog", "teacher"). The script will then generate 5 jokes using that noun as noun X and automatically select noun Y and two adjectives Z₁ and Z₂ based on the similarity criteria and subtle dataset frequency.

## How It Works

- **Word Vectors:**  
  GloVe embeddings (pre-saved as `word_vectors.kv`) are used to compute cosine similarity between words.

- **Subtle Dataset:**  
  The subtle dataset (loaded from `subtle_with_pos.csv`) is used to derive common nouns based on frequency (common_nouns). The adjectives, however, are derived directly from the filtered GloVe vocabulary (using spaCy for POS tagging).

- **Joke Generation:**  
  1. The user supplies a fixed noun (noun X).
  2. A dissimilar noun (noun Y) is selected from the common nouns (based on subtle frequency).
  3. Two adjectives (Z₁ and Z₂) are chosen from the GloVe adjectives that have high cosine similarity with both nouns.
  4. The joke is constructed using the template:  
     **"I like my X like I like my Y, Z₁ and Z₂."**

## Troubleshooting

- If you encounter errors regarding missing files, ensure that:  
  - The `word_vectors.kv` file exists and is in the correct native format.
  - The `subtle_with_pos.csv` file is correctly formatted with the expected columns.
- For issues with spaCy, verify that the English model is downloaded and installed (`en_core_web_sm`).

## References

- **GloVe:**  
  Pennington, J., Socher, R., & Manning, C. D. (2014). [GloVe: Global Vectors for Word Representation](https://nlp.stanford.edu/pubs/glove.pdf). In *EMNLP*.
- **ACL Paper (for humor generation method):**  
  [ACL Anthology Paper P13-2041](https://aclanthology.org/P13-2041.pdf)
- **spaCy:**  
  [https://spacy.io](https://spacy.io)
- **Gensim:**  
  [https://radimrehurek.com/gensim/](https://radimrehurek.com/gensim/)

---
