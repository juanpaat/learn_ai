# Hugging Face Datasets: A Deep-Dive Guide

> A guide to the 🤗 Datasets library — how to load data from anywhere, clean and transform it efficiently, work with datasets too large to fit in RAM, build your own dataset from scratch, and share it with the world.

---

## Table of Contents

1. [What Is the Datasets Library?](#1-what-is-the-datasets-library)
2. [Installing the Library](#2-installing-the-library)
3. [Loading a Dataset from the Hub](#3-loading-a-dataset-from-the-hub)
4. [Loading Data That Is Not on the Hub](#4-loading-data-that-is-not-on-the-hub)
5. [Exploring a Dataset](#5-exploring-a-dataset)
6. [Slicing — Selecting Subsets of Rows](#6-slicing--selecting-subsets-of-rows)
7. [Sorting and Shuffling](#7-sorting-and-shuffling)
8. [Filtering Rows](#8-filtering-rows)
9. [Transforming Data with map()](#9-transforming-data-with-map)
10. [Renaming, Removing, and Adding Columns](#10-renaming-removing-and-adding-columns)
11. [Converting to and from Pandas](#11-converting-to-and-from-pandas)
12. [Splitting into Train, Validation, and Test Sets](#12-splitting-into-train-validation-and-test-sets)
13. [Saving a Dataset to Disk](#13-saving-a-dataset-to-disk)
14. [Big Data — How Datasets Handles Huge Files](#14-big-data--how-datasets-handles-huge-files)
15. [Streaming — Processing Without Downloading Everything](#15-streaming--processing-without-downloading-everything)
16. [Creating Your Own Dataset from Scratch](#16-creating-your-own-dataset-from-scratch)
17. [Sharing a Dataset on the Hub](#17-sharing-a-dataset-on-the-hub)
18. [Semantic Search with Embeddings and FAISS](#18-semantic-search-with-embeddings-and-faiss)
19. [Key Takeaways](#19-key-takeaways)

---

## 1. What Is the Datasets Library?

The [🤗 Datasets library](https://github.com/huggingface/datasets) is an open-source Python package from Hugging Face. Its job is to make loading, processing, and sharing datasets as simple as possible — no matter where the data lives or how large it is.

It solves two common pain points:

| Pain point | How Datasets fixes it |
|---|---|
| Data lives in different formats and places (Hub, local CSV, remote URL, JSON…) | One consistent API — `load_dataset()` — handles all of them |
| Large datasets don't fit in RAM | Memory-mapped files backed by Apache Arrow: you can work with 100 GB datasets as if they were small |

The result is a single tool you can use from the very first exploratory notebook all the way to production training pipelines.

---

## 2. Installing the Library

```bash
uv pip install datasets

# For audio datasets:
uv pip install datasets[audio]

# For image datasets:
uv pip install datasets[vision]

# For semantic search (section 18):
uv pip install datasets faiss-cpu sentence-transformers
```

---

## 3. Loading a Dataset from the Hub

The [Hugging Face Dataset Hub](https://huggingface.co/datasets) hosts thousands of free, ready-to-use datasets. Loading one takes a single line:

```python
from datasets import load_dataset

# Load the full IMDB movie review dataset
dataset = load_dataset("imdb")
print(dataset)
```

```
DatasetDict({
    train: Dataset({features: ['text', 'label'], num_rows: 25000})
    test:  Dataset({features: ['text', 'label'], num_rows: 25000})
})
```

`load_dataset()` returns a **DatasetDict** — a dictionary where each key is a split (`"train"`, `"test"`, `"validation"`, etc.) and each value is a **Dataset** object.

### Accessing a specific split

```python
train_dataset = dataset["train"]
test_dataset  = dataset["test"]
```

### Loading only one split directly

```python
# Load only the training split — no DatasetDict, just a Dataset
train_dataset = load_dataset("imdb", split="train")
```

### Common parameters for `load_dataset()`

| Parameter | What it does |
|---|---|
| `split` | Load only one split (e.g. `"train"`, `"test"`, `"train[:10%]"`) |
| `data_files` | Path(s) or URL(s) to local or remote files |
| `data_dir` | Directory containing files (for datasets with many files) |
| `streaming` | `True` = don't download, stream on demand (see section 15) |
| `cache_dir` | Where to store downloaded files (default: `~/.cache/huggingface`) |

---

## 4. Loading Data That Is Not on the Hub

Most real-world projects have data that isn't on the Hub — CSVs, JSON files, text files, or data at a URL. `load_dataset()` handles all of these through the `data_files` parameter.

### Supported formats

| Format | `load_dataset()` type |
|---|---|
| CSV or TSV | `"csv"` |
| JSON Lines (one JSON object per line) | `"json"` |
| Plain text (one example per line) | `"text"` |
| Parquet | `"parquet"` |

### Loading a local file

```python
from datasets import load_dataset

# Single file → all rows go into the "train" split by default
dataset = load_dataset("csv", data_files="my_data.csv")

# Multiple files → still one dataset
dataset = load_dataset("json", data_files=["chunk1.jsonl", "chunk2.jsonl"])
```

### Mapping files to specific splits

```python
dataset = load_dataset(
    "csv",
    data_files={
        "train": "train.csv",
        "test":  "test.csv"
    }
)
```

### Loading from a URL (no manual downloading required)

```python
# Point data_files to a URL instead of a local path — works exactly the same
url = "https://github.com/crux82/squad-it/raw/master/SQuAD_it-train.json.gz"
dataset = load_dataset("json", data_files=url, field="data", split="train")
```

> **Automatic decompression:** if the file ends in `.gz`, `.zip`, or `.tar`, the library decompresses it automatically — no manual unzipping needed.

### Loading a local folder of files

```python
# Every file in the folder is loaded and combined into one dataset
dataset = load_dataset("imagefolder", data_dir="./my_images/")
```

---

## 5. Exploring a Dataset

Once you have a Dataset, here are the most useful ways to inspect it.

```python
from datasets import load_dataset

dataset = load_dataset("drug_reviews", split="train")
```

### Check the shape and features

```python
print(dataset)
# Dataset({features: ['drugName', 'condition', 'review', 'rating', 'date', 'usefulCount'], num_rows: 161297})

print(dataset.shape)       # (161297, 6)
print(dataset.num_rows)    # 161297
print(dataset.num_columns) # 6
print(dataset.column_names)
# ['drugName', 'condition', 'review', 'rating', 'date', 'usefulCount']
```

### Look at the first rows

```python
# Returns a dict of lists — one list per column
print(dataset[:3])

# Returns a single row as a dict
print(dataset[0])
```

### Check data types

```python
print(dataset.features)
# {'drugName': Value(dtype='string'), 'rating': Value(dtype='float64'), ...}
```

---

## 6. Slicing — Selecting Subsets of Rows

You can select specific rows using Python list-style indexing.

```python
# First 5 rows
dataset[:5]

# Last 3 rows
dataset[-3:]

# Every other row (step of 2)
dataset[::2]

# A random sample of 1000 rows (by explicit indices)
import random
indices = random.sample(range(len(dataset)), 1000)
sample = dataset.select(indices)
```

### `select()` — pick rows by a list of indices

```python
# Always the same rows — great for reproducibility
first_100 = dataset.select(range(100))

# Pick specific rows by index
custom = dataset.select([0, 42, 999, 5000])
```

---

## 7. Sorting and Shuffling

### `sort()` — order rows by a column value

```python
# Sort reviews from lowest to highest rating
sorted_dataset = dataset.sort("rating")

# Sort from highest to lowest
sorted_dataset = dataset.sort("rating", reverse=True)
```

### `shuffle()` — randomize the row order

```python
# Set a seed so you get the same shuffle every time
shuffled_dataset = dataset.shuffle(seed=42)
```

> `seed=` is important for reproducibility. Without it, every run gives a different shuffle — which can cause surprising results when debugging models.

---

## 8. Filtering Rows

`filter()` keeps only the rows where a function returns `True`. It works like Python's built-in `filter()` but on entire datasets efficiently.

```python
# Keep only reviews with a rating above 4
high_rated = dataset.filter(lambda row: row["rating"] > 4)

# Keep only rows where the condition field is not empty
clean = dataset.filter(lambda row: row["condition"] is not None)

# Filter by text length — remove very short reviews
long_reviews = dataset.filter(lambda row: len(row["review"].split()) >= 30)
```

### Filtering on multiple conditions

```python
# Use Python's "and" logic inside the lambda
filtered = dataset.filter(
    lambda row: row["rating"] >= 4 and len(row["review"].split()) >= 50
)
```

### Speeding up filtering with `num_proc`

For large datasets, you can run the filter in parallel across CPU cores:

```python
filtered = dataset.filter(
    lambda row: row["rating"] > 4,
    num_proc=4   # use 4 CPU cores
)
```

---

## 9. Transforming Data with map()

`map()` is the most important method in the library. It applies a function to every row and returns a new dataset with the result. You use it for:

- Tokenizing text
- Cleaning / normalizing strings
- Adding new computed columns
- Restructuring data

### Basic example — adding a column

```python
# Add a column with the number of words in each review
dataset = dataset.map(lambda row: {"review_length": len(row["review"].split())})

print(dataset.column_names)
# [..., 'review_length']
```

### Normalizing text

```python
def normalize(row):
    return {
        "condition": row["condition"].lower().strip() if row["condition"] else None
    }

dataset = dataset.map(normalize)
```

### Tokenizing text (the most common use case)

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")

def tokenize(row):
    return tokenizer(row["review"], truncation=True, max_length=512)

tokenized = dataset.map(tokenize)
```

### `batched=True` — make it 30x faster

By default, `map()` processes one row at a time. Setting `batched=True` sends a batch of rows (default size: 1000) to your function at once. This is dramatically faster for tokenizers:

```python
# batched=True feeds the function a dict of *lists* instead of a dict of single values
tokenized = dataset.map(tokenize, batched=True)

# You can also control the batch size
tokenized = dataset.map(tokenize, batched=True, batch_size=500)
```

> For tokenization specifically, `batched=True` can be **30x faster** because tokenizers are optimized to process text in batches.

### `num_proc` — use multiple CPU cores

```python
# Run map() in parallel across 4 CPU cores
tokenized = dataset.map(tokenize, batched=True, num_proc=4)
```

### Removing input columns you no longer need

```python
# Keep only the tokenized columns, drop everything else
tokenized = dataset.map(tokenize, batched=True, remove_columns=["review", "condition"])
```

---

## 10. Renaming, Removing, and Adding Columns

```python
# Rename a column
dataset = dataset.rename_column("Unnamed: 0", "patient_id")

# Remove one column
dataset = dataset.remove_columns(["usefulCount"])

# Remove multiple columns at once
dataset = dataset.remove_columns(["usefulCount", "date"])
```

### Adding a column computed from other columns

Use `map()` when the new value depends on the row:

```python
dataset = dataset.map(lambda row: {"word_count": len(row["review"].split())})
```

Use `Dataset.add_column()` when you already have the values as a list:

```python
import random
scores = [random.random() for _ in range(len(dataset))]
dataset = dataset.add_column("random_score", scores)
```

---

## 11. Converting to and from Pandas

Sometimes you want to use Pandas for analysis or visualization. The library makes it easy to switch back and forth.

### Convert to Pandas for analysis

```python
# Temporarily view the dataset as a Pandas DataFrame
dataset.set_format("pandas")
df = dataset[:]  # now df is a real Pandas DataFrame

# Example: count the most common conditions
print(df["condition"].value_counts().head(10))

# Reset back to the default format when done
dataset.reset_format()
```

### Create a Dataset from a Pandas DataFrame

```python
import pandas as pd
from datasets import Dataset

df = pd.read_csv("my_data.csv")
my_dataset = Dataset.from_pandas(df)
```

> When you call `Dataset.from_pandas()`, any Pandas index columns (like `__index_level_0__`) are carried over automatically. You can drop them with `dataset.remove_columns(["__index_level_0__"])`.

---

## 12. Splitting into Train, Validation, and Test Sets

```python
# Split off 10% as a test set
split = dataset.train_test_split(test_size=0.1, seed=42)

train_dataset = split["train"]
test_dataset  = split["test"]
```

### Creating a three-way split (train / validation / test)

`train_test_split()` only creates two parts. To get three, apply it twice:

```python
# Step 1: split off 20% for evaluation
split = dataset.train_test_split(test_size=0.2, seed=42)

# Step 2: split that 20% equally into validation + test
eval_split = split["test"].train_test_split(test_size=0.5, seed=42)

train      = split["train"]          # 80%
validation = eval_split["train"]     # 10%
test       = eval_split["test"]      # 10%
```

### Stratified split (preserve label proportions)

```python
# Keep the same ratio of positive/negative examples in every split
split = dataset.train_test_split(test_size=0.2, stratify_by_column="label", seed=42)
```

---

## 13. Saving a Dataset to Disk

After all your cleaning and processing work, save the result so you don't have to repeat it next time.

### Arrow format (fastest for reloading)

```python
# Save
dataset.save_to_disk("my_processed_dataset")

# Load back
from datasets import load_from_disk
dataset = load_from_disk("my_processed_dataset")
```

Arrow is the default internal format. Saving and loading in Arrow is nearly instant compared to CSV or JSON.

### CSV

```python
dataset.to_csv("my_dataset.csv", index=False)

# Reload
dataset = load_dataset("csv", data_files="my_dataset.csv", split="train")
```

### JSON Lines

```python
dataset.to_json("my_dataset.jsonl")

# Reload
dataset = load_dataset("json", data_files="my_dataset.jsonl", split="train")
```

---

## 14. Big Data — How Datasets Handles Huge Files

Most Python data tools (like Pandas) load the entire file into RAM. A 10 GB file requires ~50 GB of RAM once you factor in copies and transformations. This makes large-scale NLP impractical on normal machines.

The Datasets library solves this with **memory mapping** backed by the [Apache Arrow](https://arrow.apache.org/) format.

### How memory mapping works

Instead of loading everything into RAM, the library maps the file to virtual memory. The operating system loads only the pages you actually access — so browsing a 100 GB dataset uses almost no RAM until you start reading specific rows.

```python
from datasets import load_dataset
import psutil, os

# Load a massive dataset — this does NOT load everything into RAM
dataset = load_dataset("the_pile", split="train")  # 825 GB total

# Check actual RAM usage — it stays low
process = psutil.Process(os.getpid())
print(f"RAM used: {process.memory_info().rss / 1e9:.2f} GB")
# RAM used: ~0.8 GB — the data is not in memory, only mapped
```

### Why this matters in practice

| Approach | 20 GB dataset RAM requirement |
|---|---|
| Pandas `read_csv()` | ~100 GB (5x multiplier is typical) |
| 🤗 Datasets | ~1–2 GB (only the pages you touch) |

---

## 15. Streaming — Processing Without Downloading Everything

Memory mapping still requires the files to be on disk. If a dataset is 800 GB and you only have 50 GB of free storage, you are stuck — unless you stream.

**Streaming** downloads data on demand, one example at a time, without ever saving the full dataset locally. You get an `IterableDataset` instead of a `Dataset`.

### When to use streaming

- The dataset is larger than your available disk space
- You want to start training immediately without waiting for a full download
- You are exploring a huge dataset and only need a sample

### Loading a dataset in streaming mode

```python
from datasets import load_dataset

# streaming=True → returns an IterableDataset, not a Dataset
streamed = load_dataset("the_pile", split="train", streaming=True)

print(type(streamed))
# <class 'datasets.iterable_dataset.IterableDataset'>
```

### Iterating over a streamed dataset

```python
# You loop over it — each iteration downloads the next batch
for example in streamed:
    print(example["text"][:100])
    break
```

### `take()` — grab the first N examples

```python
# Take the first 5 examples — only those 5 are downloaded
sample = list(streamed.take(5))
print(sample[0]["text"])
```

### `skip()` — skip the first N examples

```python
# Skip the first 1000, then take the next 10
subset = streamed.skip(1000).take(10)
```

### `map()` on a streamed dataset

```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")

def tokenize(batch):
    return tokenizer(batch["text"], truncation=True, max_length=128)

# Transformations are applied lazily — nothing is downloaded yet
tokenized_stream = streamed.map(tokenize, batched=True)

# Data is only downloaded + tokenized when you iterate
for batch in tokenized_stream.take(100):
    pass
```

### Shuffling a streamed dataset

You can't shuffle an infinite stream globally, but you can use a **buffer**: the library keeps N examples in memory and picks from them randomly.

```python
shuffled = streamed.shuffle(seed=42, buffer_size=10_000)
```

### Combining multiple datasets into one stream

```python
from datasets import interleave_datasets

# Load two sources as streams
pubmed  = load_dataset("pubmed", split="train", streaming=True)
freelaw = load_dataset("free_law", split="train", streaming=True)

# Interleave them — examples alternate between the two sources
combined = interleave_datasets([pubmed, freelaw])
```

### IterableDataset vs Dataset — quick comparison

| Feature | `Dataset` (normal) | `IterableDataset` (streaming) |
|---|---|---|
| Download on load | Full download | No download |
| Disk space needed | Full size | Almost none |
| Random access (`dataset[42]`) | Yes | No |
| `select()` / `sort()` | Yes | No |
| `filter()` / `map()` | Yes (fast) | Yes (lazy) |
| `take()` / `skip()` | No | Yes |
| Best for | Datasets that fit on disk | Datasets larger than your disk |

---

## 16. Creating Your Own Dataset from Scratch

You don't have to use existing datasets. Here is how to build one yourself and prepare it for use.

### From a Pandas DataFrame

The easiest path: do your data collection in Pandas, then convert.

```python
import pandas as pd
from datasets import Dataset

df = pd.DataFrame({
    "text":  ["I love this product.", "Terrible experience.", "It was okay."],
    "label": [1, 0, 1]
})

dataset = Dataset.from_pandas(df)
print(dataset)
# Dataset({features: ['text', 'label'], num_rows: 3})
```

### From a dictionary of lists

```python
from datasets import Dataset

data = {
    "question": ["What is 2+2?", "Capital of France?"],
    "answer":   ["4",            "Paris"]
}

dataset = Dataset.from_dict(data)
```

### From a list of dicts (JSON-style records)

```python
from datasets import Dataset

records = [
    {"text": "First example", "label": 0},
    {"text": "Second example", "label": 1},
]

dataset = Dataset.from_list(records)
```

### Practical example — collecting GitHub issues via API

Here is a real-world workflow: fetch issues from a GitHub repo, clean them, and build a dataset for training a search engine.

**Step 1 — Fetch data from the API**

```python
import requests, time, math
import pandas as pd
from pathlib import Path
from tqdm import tqdm

GITHUB_TOKEN = "your_token_here"   # from github.com/settings/tokens
headers = {"Authorization": f"token {GITHUB_TOKEN}"}

def fetch_issues(owner="huggingface", repo="datasets", num_issues=1000):
    all_issues = []
    per_page = 100
    num_pages = math.ceil(num_issues / per_page)

    for page in tqdm(range(num_pages)):
        url = f"https://api.github.com/repos/{owner}/{repo}/issues"
        params = {"page": page, "per_page": per_page, "state": "all"}
        issues = requests.get(url, headers=headers, params=params).json()
        all_issues.extend(issues)

    return all_issues

issues = fetch_issues()
df = pd.DataFrame(issues)
df.to_json("datasets-issues.jsonl", orient="records", lines=True)
```

**Step 2 — Load into a Dataset**

```python
from datasets import load_dataset

issues_dataset = load_dataset("json", data_files="datasets-issues.jsonl", split="train")
print(issues_dataset)
```

**Step 3 — Clean: separate issues from pull requests**

```python
# The API returns both issues and PRs; pull requests have a non-null "pull_request" field
issues_dataset = issues_dataset.map(
    lambda row: {"is_pull_request": row["pull_request"] is not None}
)

# Keep only real issues
issues_only = issues_dataset.filter(lambda row: not row["is_pull_request"])
```

**Step 4 — Augment: fetch the comments for each issue**

```python
def get_comments(issue_number):
    url = f"https://api.github.com/repos/huggingface/datasets/issues/{issue_number}/comments"
    return [c["body"] for c in requests.get(url, headers=headers).json()]

issues_with_comments = issues_only.map(
    lambda row: {"comments": get_comments(row["number"])}
)
```

---

## 17. Sharing a Dataset on the Hub

Once your dataset is ready, sharing it takes one line. Anyone in the world can then load it with `load_dataset("your-username/dataset-name")`.

### Log in first

```python
# In a notebook:
from huggingface_hub import notebook_login
notebook_login()
```

```bash
# In a terminal:
huggingface-cli login
```

### Push to the Hub

```python
# This creates the repo and uploads all splits
dataset.push_to_hub("your-username/my-dataset")

# Push a specific split under a custom name
train_dataset.push_to_hub("your-username/my-dataset", split="train")

# Make it private
dataset.push_to_hub("your-username/my-dataset", private=True)
```

### Load it back from the Hub

```python
from datasets import load_dataset

dataset = load_dataset("your-username/my-dataset", split="train")
```

### Writing a dataset card

A dataset card is the README on your dataset's Hub page. It helps others understand what the data contains, how it was collected, and what it can be used for. Well-documented datasets get far more usage.

Add a `README.md` to the dataset repository on the Hub with at minimum:
- What the dataset contains
- How it was collected
- Intended uses and known limitations
- Citation information

---

## 18. Semantic Search with Embeddings and FAISS

This section ties together everything: you take a cleaned dataset, create embeddings (numerical representations of meaning), and index them so you can search by meaning instead of keywords.

**When to use this:** you have a large collection of documents and want to find the ones most relevant to a query — without needing exact keyword matches.

### The idea

1. Convert each document into an embedding vector using a language model
2. Index all the vectors with FAISS (a fast nearest-neighbour library from Meta)
3. At query time, embed the query and find the closest vectors in the index

### Step 1 — Load and prepare the data

```python
from datasets import load_dataset

dataset = load_dataset("lewtun/github-issues", split="train")

# Keep only real issues with meaningful comments
dataset = dataset.filter(
    lambda row: (not row["is_pull_request"])
                and len(row["comments"]) > 0
)

# Combine title, body, and comments into a single text field for embedding
def concatenate_text(row):
    return {
        "text": row["title"] + "\n" + row["body"] + "\n" + " ".join(row["comments"])
    }

dataset = dataset.map(concatenate_text)
```

### Step 2 — Generate embeddings

```python
import torch
from transformers import AutoTokenizer, AutoModel

# This model is optimized for semantic similarity / search
model_name = "sentence-transformers/multi-qa-mpnet-base-dot-v1"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModel.from_pretrained(model_name)

def cls_pooling(model_output):
    # CLS token is the first token — it represents the whole sentence
    return model_output.last_hidden_state[:, 0]

def get_embeddings(texts):
    encoded = tokenizer(
        texts, padding=True, truncation=True, max_length=512, return_tensors="pt"
    )
    with torch.no_grad():
        output = model(**encoded)
    return cls_pooling(output).numpy()

# Add embeddings to the dataset
dataset = dataset.map(
    lambda batch: {"embeddings": get_embeddings(batch["text"])},
    batched=True,
    batch_size=32
)
```

### Step 3 — Build the FAISS index

```python
# Tell the dataset to build a nearest-neighbor index on the "embeddings" column
dataset.add_faiss_index(column="embeddings")
```

This happens in memory and takes a few seconds on small datasets (minutes on large ones).

### Step 4 — Query the index

```python
# Embed your question the same way
question = "How can I load a dataset offline?"
question_embedding = get_embeddings([question])

# Find the 5 most similar documents
scores, results = dataset.get_nearest_examples("embeddings", question_embedding, k=5)

for score, text in zip(scores, results["text"]):
    print(f"Score: {score:.2f}")
    print(text[:200])
    print("---")
```

### Step 5 — Save and reload the index

```python
# Save the dataset with the index embedded
dataset.save_faiss_index("embeddings", "my_index.faiss")

# Load it back later — no need to recompute embeddings
dataset.load_faiss_index("embeddings", "my_index.faiss")
```

### Full picture of what just happened

```
Your corpus of documents
    ↓  get_embeddings()
768-dimensional vectors (one per document)
    ↓  add_faiss_index()
FAISS index (fast nearest-neighbor lookup)
    ↓  get_nearest_examples(question_embedding, k=5)
Top-5 most semantically similar documents
```

---

## 19. Key Takeaways

| Concept | Plain-English Explanation |
|---|---|
| `load_dataset("name")` | Load any dataset from the Hub in one line |
| `data_files=` | Load local files (CSV, JSON, text) or remote URLs with the same function |
| `split=` | Load only one part of a dataset (e.g. `"train"`, `"test"`) |
| `Dataset` | A table-like object backed by Apache Arrow — fast and memory-efficient |
| `DatasetDict` | A dict of `Dataset` objects, one per split |
| `filter()` | Keep only rows that pass a condition |
| `map()` | Apply a transformation to every row — the core tool for cleaning and tokenizing |
| `batched=True` | Pass rows in batches to `map()` — makes tokenization ~30x faster |
| `num_proc=` | Run `filter()` or `map()` in parallel on multiple CPU cores |
| `train_test_split()` | Split a dataset into train and test parts |
| `save_to_disk()` / `load_from_disk()` | Save processed datasets so you don't redo work |
| Memory mapping | How Datasets lets you work with 100 GB files on a laptop — only loaded pages use RAM |
| `streaming=True` | Process datasets larger than your disk — downloads examples on demand |
| `IterableDataset` | The streaming version of a Dataset — supports `take()`, `skip()`, lazy `map()` |
| `interleave_datasets()` | Combine multiple streaming sources into one alternating stream |
| `push_to_hub()` | Share your dataset publicly (or privately) on the Hugging Face Hub |
| `add_faiss_index()` | Build a nearest-neighbor index on an embeddings column for semantic search |
| `get_nearest_examples()` | Query the FAISS index to find the most semantically similar documents |
