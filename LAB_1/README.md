# Hindi Corpus Tokenization and Corpus Statistics

## Overview

This project builds a complete preprocessing pipeline for a large-scale Hindi corpus using the **IndicCorpV2** dataset from AI4Bharat.

The pipeline performs:

- Sentence Tokenization
- Word Tokenization using Regular Expressions
- Tokenized corpus generation
- Parquet dataset creation
- Corpus statistics calculation
- Visualization of corpus statistics

The implementation processes **1,000,000 streamed documents** from the IndicCorpV2 Hindi corpus.

---

# Dataset

**Dataset Name**

- AI4Bharat IndicCorpV2

**Language**

- Hindi (Devanagari)

**HuggingFace Dataset**

https://huggingface.co/datasets/ai4bharat/IndicCorpV2

Dataset is loaded in streaming mode to avoid downloading the complete corpus.

```python
dataset = load_dataset(
    "ai4bharat/IndicCorpV2",
    "indiccorp_v2",
    split="hin_Deva",
    streaming=True
)
```

---

# Project Structure

```
project/
│
├── output/
│   ├── tokenized_sentences.txt
│   └── tokenized_sentences.parquet
│
├── tokenizer.py
├── README.md
```

---

# Libraries Used

```python
re
os
pandas
datasets
matplotlib
```

Install dependencies

```bash
pip install pandas matplotlib datasets pyarrow
```

---

# Processing Pipeline

The pipeline consists of six major stages.

---

## 1. Load Dataset

The Hindi corpus is streamed directly from HuggingFace.

Advantages

- Low memory usage
- No local dataset download
- Scalable to very large datasets

---

## 2. Sentence Tokenization

Each paragraph is divided into individual sentences.

Sentence delimiters:

- ।
- !
- ?

Implementation

```python
re.split(r'(?<=[।!?])\s+', paragraph)
```

Example

Input

```
आज मौसम अच्छा है।
मैं घर जा रहा हूँ!
```

Output

```
आज मौसम अच्छा है।
मैं घर जा रहा हूँ!
```

---

## 3. Word Tokenization

The tokenizer is implemented entirely using **Regular Expressions**.

It recognizes:

### URLs

Example

```
https://huggingface.co
```

---

### Email Addresses

Example

```
abc@gmail.com
```

---

### Dates

Supported formats

```
15/08/2026

15-08-2026
```

---

### Numbers

Supports

```
25

25.75
```

---

### Hindi Words

Unicode range

```
\u0900-\u097F
```

Examples

```
भारत

विद्यालय

मौसम
```

---

### English Words

Examples

```
Python

NLP

Google
```

---

### Punctuation

Recognizes

```
।

!

?

.

,

;

:

"

()

[]

{}
```

---

## Token Pattern

```python
TOKEN_PATTERN = re.compile(
    URL_PATTERN
    | EMAIL_PATTERN
    | DATE_PATTERN
    | NUMBER_PATTERN
    | Hindi Words
    | English Words
    | Punctuation
)
```

---

# Example

Input

```
आज मौसम अच्छा है।
मेरा ईमेल abc@gmail.com है।
आज तारीख 15/08/2026 है।
कीमत 25.75 रुपये है!
```

Output

```
आज
मौसम
अच्छा
है

मेरा
ईमेल
abc@gmail.com
है

आज
तारीख
15/08/2026
है

कीमत
25.75
रुपये
है
!
```

---

# Processing

The program processes

```
1,000,000
```

documents.

Progress is displayed every

```
100000
```

documents.

Example

```
Processed 100000 rows...

Processed 200000 rows...

...

Processed 1000000 rows...
```

---

# Output Files

## 1. Tokenized Text File

```
output/tokenized_sentences.txt
```

Each sentence is stored on a separate line.

Tokens are separated using

```
#
```

Example

```
आज#मौसम#अच्छा#है
```

---

## 2. Parquet File

```
output/tokenized_sentences.parquet
```

Each row contains one tokenized sentence.

Example

| sentence |
|-----------|
| आज मौसम अच्छा है |
| मेरा ईमेल abc@gmail.com है |

The Parquet format provides:

- Fast loading
- Efficient compression
- Compatibility with machine learning workflows

---

# Corpus Statistics

The following statistics are computed.

### Total Sentences

Number of tokenized sentences.

---

### Total Words

Total number of tokens generated.

---

### Total Characters

Total number of characters across all tokens.

---

### Average Sentence Length

Formula

```
Total Words / Total Sentences
```

---

### Average Word Length

Formula

```
Total Characters / Total Words
```

---

### Unique Words

Vocabulary size of the corpus.

---

### Type Token Ratio (TTR)

Formula

```
Unique Words / Total Words
```

TTR measures lexical diversity.

Higher TTR indicates a more diverse vocabulary.

---

# Results

| Statistic | Value |
|------------|------------:|
| Documents Processed | 1,000,000 |
| Total Sentences | 1,420,490 |
| Total Words | 30,245,176 |
| Total Characters | 117,726,814 |
| Average Sentence Length | 21.29 |
| Average Word Length | 3.89 |
| Unique Words | 401,166 |
| Type Token Ratio | 0.01326 |

---

# Visualizations

The project generates three plots.

## 1. Corpus Statistics Bar Chart

Displays

- Total Sentences
- Total Words
- Total Characters
- Unique Words

---

## 2. Average Statistics Bar Chart

Displays

- Average Sentence Length
- Average Word Length
- Type Token Ratio

---

## 3. Vocabulary Composition Pie Chart

Shows

- Unique Words
- Repeated Words

---

# Applications

The generated corpus can be used for

- NLP preprocessing
- Language modeling
- Tokenizer training
- Vocabulary analysis
- Word frequency analysis
- Transformer model pretraining
- Text classification
- Named Entity Recognition (NER)
- Machine Translation
- Sentiment Analysis

---

# Future Improvements

Possible enhancements include

- Stop-word removal
- Unicode normalization
- Stemming
- Lemmatization
- Byte Pair Encoding (BPE)
- WordPiece tokenizer
- SentencePiece tokenizer
- Subword tokenization
- Named Entity Recognition support
- Frequency-based vocabulary generation

---

# Author

Hindi Corpus Tokenization Project

Built using

- Python
- Regular Expressions
- HuggingFace Datasets
- Pandas
- Matplotlib