# Hugging Face Pipelines: A Deep-Dive Guide

> A detailed, plain-English guide to every pipeline task in the 🤗 Transformers library — with real examples, the most important parameters for each task, how to find and swap models, and whether you can plug in OpenAI, Anthropic, or Gemini instead.

---

## Table of Contents

1. [What Is the Transformers Library?](#1-what-is-the-transformers-library)
2. [Installing the Library](#2-installing-the-library)
3. [The Pipeline Function — How It Works](#3-the-pipeline-function--how-it-works)
4. [How to Use a Custom Model Instead of the Default](#4-how-to-use-a-custom-model-instead-of-the-default)
5. [How to Find Models on the Hugging Face Hub](#5-how-to-find-models-on-the-hugging-face-hub)
6. [Common Parameters That Work on Every Pipeline](#6-common-parameters-that-work-on-every-pipeline)
7. [Task 1 — Sentiment Analysis](#7-task-1--sentiment-analysis)
8. [Task 2 — Zero-Shot Classification](#8-task-2--zero-shot-classification)
9. [Task 3 — Text Generation](#9-task-3--text-generation)
10. [Task 4 — Text2Text Generation](#10-task-4--text2text-generation)
11. [Task 5 — Fill-Mask](#11-task-5--fill-mask)
12. [Task 6 — Named Entity Recognition (NER)](#12-task-6--named-entity-recognition-ner)
13. [Task 7 — Question Answering](#13-task-7--question-answering)
14. [Task 8 — Summarization](#14-task-8--summarization)
15. [Task 9 — Translation](#15-task-9--translation)
16. [Task 10 — Feature Extraction](#16-task-10--feature-extraction)
17. [Task 11 — Image Classification](#17-task-11--image-classification)
18. [Task 12 — Automatic Speech Recognition](#18-task-12--automatic-speech-recognition)
19. [Can You Use OpenAI, Anthropic, or Gemini with This Library?](#19-can-you-use-openai-anthropic-or-gemini-with-this-library)
20. [Key Takeaways](#20-key-takeaways)

---

## 1. What Is the Transformers Library?

The [🤗 Transformers library](https://github.com/huggingface/transformers) is an open-source Python package built by Hugging Face. It lets you download, run, and fine-tune pre-trained AI models for text, images, audio, and more — all in a few lines of code.

The models live on the [Hugging Face Model Hub](https://huggingface.co/models), a public repository with hundreds of thousands of free models. You pick one, load it, and run it. That's it.

---

## 2. Installing the Library

```bash
pip install transformers

# If you want GPU support via PyTorch (recommended for speed):
pip install transformers torch

# If you prefer TensorFlow:
pip install transformers tensorflow
```

---

## 3. The Pipeline Function — How It Works

`pipeline()` is the highest-level, simplest way to use any model. It wraps three steps automatically:

```
Your text  →  [Tokenizer]  →  [Model]  →  [Postprocessor]  →  Result
```

| Step | What happens |
|---|---|
| **Tokenizer** | Converts your raw text into numbers (tokens) the model understands |
| **Model** | Runs the neural network on those numbers and produces raw outputs |
| **Postprocessor** | Converts the raw outputs into something human-readable: a label, score, or sentence |

You never touch any of those steps manually when using `pipeline()`. They all happen behind the scenes.

```python
from transformers import pipeline

# The simplest possible call
classifier = pipeline("sentiment-analysis")
classifier("I love this library!")
# [{'label': 'POSITIVE', 'score': 0.9998}]
```

The first time you run this, the model is downloaded from the Hub and cached locally on your machine. Every subsequent run uses the local cache — no internet connection required.

---

## 4. How to Use a Custom Model Instead of the Default

Every pipeline task has a default model picked by Hugging Face. These defaults are solid general-purpose models, but you will often want something more specialized — a model trained on medical text, a smaller/faster model, a model in a specific language, etc.

You swap the model using the `model=` argument:

```python
from transformers import pipeline

# Swap in any model from the Hub by its model ID
generator = pipeline("text-generation", model="distilgpt2")
```

The `model=` value is the **model ID** — the string you see in the URL on the Hub. For example:

| Hub URL | Model ID to use |
|---|---|
| `huggingface.co/distilgpt2` | `"distilgpt2"` |
| `huggingface.co/Helsinki-NLP/opus-mt-fr-en` | `"Helsinki-NLP/opus-mt-fr-en"` |
| `huggingface.co/cardiffnlp/twitter-roberta-base-sentiment` | `"cardiffnlp/twitter-roberta-base-sentiment"` |

### Controlling which device runs the model

```python
# Run on CPU (default)
pipe = pipeline("text-generation", model="distilgpt2", device=-1)

# Run on the first GPU (much faster for large models)
pipe = pipeline("text-generation", model="distilgpt2", device=0)

# Automatically pick the best device (GPU if available, otherwise CPU)
pipe = pipeline("text-generation", model="distilgpt2", device_map="auto")
```

### Controlling memory usage with quantization

Large models can require a lot of RAM. You can load them in reduced precision to use less memory:

```python
from transformers import pipeline, BitsAndBytesConfig

# Load in 8-bit (requires: pip install bitsandbytes)
pipe = pipeline(
    "text-generation",
    model="mistralai/Mistral-7B-v0.1",
    model_kwargs={"load_in_8bit": True},
    device_map="auto"
)
```

---

## 5. How to Find Models on the Hugging Face Hub

The Hub is at [huggingface.co/models](https://huggingface.co/models). Here is the step-by-step process for finding the right model:

### Step 1 — Filter by task

On the left sidebar, click the task you need (e.g., "Text Classification", "Translation", "Summarization"). This hides all models that are not designed for that task.

### Step 2 — Filter by language

If you need a model for a specific language (Spanish, German, Chinese, etc.), use the **Language** filter. Many tasks have models for dozens of languages.

### Step 3 — Sort by downloads or likes

Sort by **Most Downloads** to find the most battle-tested models used by the community. Sort by **Most Likes** to find community favorites.

### Step 4 — Read the model card

Click any model to open its **model card** — a description page that explains:
- What the model was trained on
- What tasks it handles
- Expected input/output format
- Known limitations

### Step 5 — Try the live widget

Every model page has an **Inference Widget** on the right side. Type some text and click "Compute" to see the model's output live, without writing any code. This is the fastest way to test whether a model suits your needs.

### Step 6 — Copy the model ID

Once you are happy with a model, copy its ID from the URL bar (e.g., `cardiffnlp/twitter-roberta-base-sentiment`) and paste it into `pipeline(model="...")`.

---

## 6. Common Parameters That Work on Every Pipeline

These parameters apply regardless of which task you use:

| Parameter | Type | What it does |
|---|---|---|
| `model` | `str` | The model ID from the Hub to use instead of the default |
| `tokenizer` | `str` | Override the tokenizer separately from the model |
| `device` | `int` | `-1` for CPU, `0` for first GPU, `1` for second GPU, etc. |
| `device_map` | `str` | `"auto"` to automatically pick the best device |
| `batch_size` | `int` | How many inputs to process at once (speeds up large jobs) |
| `model_kwargs` | `dict` | Extra arguments passed directly to the model loader |

```python
pipe = pipeline(
    "summarization",
    model="facebook/bart-large-cnn",
    device=0,          # GPU
    batch_size=8       # Process 8 texts at once
)
```

---

## 7. Task 1 — Sentiment Analysis

**What it does:** Reads a piece of text and decides whether the expressed feeling is positive, negative, or neutral. Useful for analyzing product reviews, social media posts, or customer feedback.

**Returns:** `list[dict]` — one dict per input, with keys `label` (string) and `score` (float 0–1).

```python
result = classifier("Great product!")
# [{'label': 'POSITIVE', 'score': 0.9997}]

result[0]["label"]   # 'POSITIVE'
result[0]["score"]   # 0.9997
```

### Basic example

```python
from transformers import pipeline

classifier = pipeline("sentiment-analysis")

classifier("I absolutely love this product, it changed my life!")
# [{'label': 'POSITIVE', 'score': 0.9997}]

classifier("The delivery was late and the item was broken.")
# [{'label': 'NEGATIVE', 'score': 0.9991}]
```

### Processing multiple texts at once

```python
results = classifier([
    "Great experience overall.",
    "Terrible customer service.",
    "It was okay, nothing special."
])

for r in results:
    print(r["label"], round(r["score"], 3))
# POSITIVE 0.998
# NEGATIVE 0.999
# POSITIVE 0.668
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `top_k` | `1` | Return the top N labels instead of just the best one |
| `truncation` | `False` | `True` = silently cut the input at the model's limit if it is too long. `False` = raise an error when input exceeds the limit |
| `max_length` | model's limit | Maximum number of tokens to process |

```python
# Get scores for all labels, not just the top one
classifier("It was decent.", top_k=None)
# [{'label': 'POSITIVE', 'score': 0.72}, {'label': 'NEGATIVE', 'score': 0.28}]
```

### Recommended specialized models

| Use case | Model ID |
|---|---|
| Twitter/social media | `cardiffnlp/twitter-roberta-base-sentiment-latest` |
| Finance / stocks | `ProsusAI/finbert` |
| Multi-language | `lxyuan/distilbert-base-multilingual-cased-sentiments-student` |
| More fine-grained (5 stars) | `nlptown/bert-base-multilingual-uncased-sentiment` |

```python
# Example with a finance-specific model
fin_classifier = pipeline(
    "sentiment-analysis",
    model="ProsusAI/finbert"
)
fin_classifier("The company's revenue grew 40% year over year.")
# [{'label': 'positive', 'score': 0.978}]
```

---

## 8. Task 2 — Zero-Shot Classification

**What it does:** Classifies text into categories that you define at runtime — without any training data. The model has never seen your labels during training; it uses its deep language understanding to figure out which label fits best.

This is a game-changer for real-world projects where you don't have labeled data or where your categories change frequently.

**Returns:** `dict` (not a list) — a single dict with keys `sequence` (the original text), `labels` (list of strings sorted by score), and `scores` (list of floats in the same order).

```python
result = classifier("Some text", candidate_labels=["a", "b"])
# {'sequence': 'Some text', 'labels': ['a', 'b'], 'scores': [0.85, 0.15]}

result["labels"][0]   # best label → 'a'
result["scores"][0]   # its score  → 0.85
```

### Basic example

```python
from transformers import pipeline

classifier = pipeline("zero-shot-classification")

classifier(
    "The government announced new tax reforms for small businesses.",
    candidate_labels=["politics", "economics", "sports", "technology"]
)
```

```python
{
  'sequence': 'The government announced new tax reforms for small businesses.',
  'labels':   ['economics', 'politics', 'technology', 'sports'],
  'scores':   [0.612,        0.301,      0.068,         0.019]
}
```

The labels are always returned sorted from most to least likely.

### Multi-label mode

By default the model assumes only one label applies. If multiple labels can be true at the same time, set `multi_label=True`:

```python
classifier(
    "The new smartphone has a revolutionary camera and great battery life.",
    candidate_labels=["camera", "battery", "design", "price"],
    multi_label=True
)
# {'labels': ['camera', 'battery', 'design', 'price'],
#  'scores': [0.98, 0.95, 0.42, 0.11]}
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `candidate_labels` | required | List of strings — the categories to classify into |
| `multi_label` | `False` | `True` = multiple labels can apply at the same time, each scored independently. `False` = scores are compared against each other and must sum to 1 (only the best label wins) |
| `hypothesis_template` | `"This example is {}."` | The template used to form the hypothesis for each label |

### Customizing the hypothesis template

The model internally turns your task into a textual entailment problem. By default it checks "This example is {label}." You can change this template to make it more natural for your domain:

```python
classifier(
    "The patient has a fever and a persistent cough.",
    candidate_labels=["flu", "allergy", "covid"],
    hypothesis_template="The patient is suffering from {}."
)
```

### Recommended models

| Model ID | Notes |
|---|---|
| `facebook/bart-large-mnli` | Default, strong general-purpose model |
| `cross-encoder/nli-deberta-v3-base` | More accurate, a bit slower |
| `MoritzLaurer/deberta-v3-large-zeroshot-v2` | State-of-the-art zero-shot |

---

## 9. Task 3 — Text Generation

**What it does:** You give the model the beginning of a text (called a **prompt**), and it continues writing from there. This is the core capability behind ChatGPT and similar tools — though here you're using smaller, open models.

**Returns:** `list[dict]` — one dict per generated sequence, with a single key `generated_text` (string) that contains the full text including your original prompt.

```python
result = generator("Once upon a time")
# [{'generated_text': 'Once upon a time there was a princess who...'}]

result[0]["generated_text"]   # the full generated string
```

### Basic example

```python
from transformers import pipeline

generator = pipeline("text-generation")

generator("Once upon a time, in a land far away,")
```

```python
[{'generated_text': 'Once upon a time, in a land far away, there was a kingdom ruled by a wise king who...'}]
```

> Text generation involves randomness. You will get a different result every time.

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `max_length` | `50` | Maximum total tokens in the output (prompt + generated text) |
| `max_new_tokens` | `None` | Maximum number of *new* tokens to generate (ignores prompt length) |
| `min_length` | `0` | Minimum total tokens in the output |
| `num_return_sequences` | `1` | How many different completions to generate |
| `do_sample` | `False` | `True` = picks the next token randomly from the probability distribution (more creative, different result every run). `False` = always picks the single most likely next token (deterministic, same result every run) |
| `temperature` | `1.0` | Controls how bold the random choices are. Only has effect when `do_sample=True`. Lower (e.g. `0.3`) = safe, focused output. Higher (e.g. `1.5`) = risky, more creative output |
| `top_k` | `50` | At each step, only consider the top K most likely tokens and ignore the rest. Only has effect when `do_sample=True` |
| `top_p` | `1.0` | Also called nucleus sampling. `1.0` = consider all tokens. Lower values (e.g. `0.9`) = only consider the smallest set of tokens whose combined probability reaches 90%, ignoring unlikely ones. Only has effect when `do_sample=True` |
| `repetition_penalty` | `1.0` | `1.0` = no penalty (tokens can repeat freely). Values above `1.0` (e.g. `1.3`) = make it less likely to repeat a token that already appeared |
| `truncation` | `False` | `True` = silently cut the prompt at the model's context limit if it is too long. `False` = raise an error when the input is too long |

### The most important parameters in plain English

- **`max_new_tokens`** — prefer this over `max_length` because `max_length` counts the prompt tokens too, which can be confusing.
- **`temperature`** — think of it as a "creativity dial". Set `0.1` for factual, predictable output. Set `1.2` for more varied, imaginative output.
- **`do_sample=True`** — must be set to `True` for `temperature`, `top_k`, and `top_p` to have any effect. Without it, the model always picks the single most likely next word.

### Practical example with parameters

```python
generator = pipeline("text-generation", model="distilgpt2")

results = generator(
    "The future of artificial intelligence is",
    max_new_tokens=60,
    num_return_sequences=3,
    do_sample=True,
    temperature=0.8,
    top_p=0.9,
    repetition_penalty=1.2
)

for i, r in enumerate(results):
    print(f"--- Option {i+1} ---")
    print(r["generated_text"])
```

### Recommended models

| Model ID | Notes |
|---|---|
| `distilgpt2` | Small, fast, good for testing |
| `gpt2` | Classic OpenAI GPT-2 (open weights) |
| `gpt2-medium` | Larger, better quality |
| `mistralai/Mistral-7B-Instruct-v0.2` | Excellent 7B instruction-following model |
| `meta-llama/Meta-Llama-3-8B-Instruct` | Very capable, requires accepting license on Hub |
| `microsoft/phi-2` | Small but surprisingly capable |

---

## 10. Task 4 — Text2Text Generation

**What it does:** Similar to text generation, but designed for models that were trained to take one piece of text and produce a *different* piece of text in response — like T5 and FLAN-T5. These models are trained on many tasks simultaneously (summarization, translation, question answering, etc.) using task prefixes.

**Returns:** `list[dict]` — one dict with a single key `generated_text` (string) containing the model's output (not the input — only the newly generated part).

```python
result = pipe("Translate English to French: Hello")
# [{'generated_text': 'Bonjour'}]

result[0]["generated_text"]   # 'Bonjour'
```

### Basic example

```python
from transformers import pipeline

# FLAN-T5 can handle many tasks using natural language instructions
pipe = pipeline("text2text-generation", model="google/flan-t5-base")

# Translation
pipe("Translate English to French: The weather is beautiful today.")
# [{'generated_text': "Il fait très beau aujourd'hui."}]

# Summarization
pipe("Summarize: Machine learning is a branch of AI that allows systems to learn from data.")
# [{'generated_text': 'Machine learning allows systems to learn from data.'}]

# Question answering
pipe("Answer: What is the capital of France? Context: France is a country in Europe. Its capital is Paris.")
# [{'generated_text': 'Paris'}]
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `max_length` | `20` | Maximum tokens in the output |
| `max_new_tokens` | `None` | Max new tokens to generate |
| `num_beams` | `1` | Number of beams for beam search. Higher = better quality, slower |
| `early_stopping` | `False` | `True` = stop beam search as soon as all beams have produced an end-of-sequence token (faster). `False` = keep going until the full `max_length` is reached. Only useful when `num_beams > 1` |
| `no_repeat_ngram_size` | `0` | `0` = no restriction. Any value above `0` (e.g. `3`) = the model is forbidden from repeating any sequence of that many words |

```python
pipe(
    "Summarize: " + long_article_text,
    max_length=100,
    num_beams=4,
    early_stopping=True,
    no_repeat_ngram_size=3
)
```

### Recommended models

| Model ID | Notes |
|---|---|
| `google/flan-t5-base` | Small instruction-following model |
| `google/flan-t5-large` | Better quality, more memory |
| `google/flan-t5-xl` | Even better, needs a GPU |
| `google/flan-t5-xxl` | Best quality, needs significant GPU memory |

---

## 11. Task 5 — Fill-Mask

**What it does:** The model predicts what word should fill a blank in a sentence. You mark the blank with the special token `<mask>` (or `[MASK]` depending on the model). This is how models like BERT were originally trained.

**Returns:** `list[dict]` — one dict per candidate word, sorted from most to least likely. Each dict has keys `sequence` (full sentence with the blank filled), `score` (float), `token` (integer token id), and `token_str` (the predicted word as a string).

```python
result = unmasker("Paris is the <mask> of France.", top_k=2)
# [
#   {'sequence': 'Paris is the capital of France.', 'score': 0.979, 'token': 1007, 'token_str': 'capital'},
#   {'sequence': 'Paris is the heart of France.',   'score': 0.005, 'token': 2540, 'token_str': 'heart'}
# ]

result[0]["token_str"]   # 'capital'
result[0]["score"]       # 0.979
```

### Basic example

```python
from transformers import pipeline

unmasker = pipeline("fill-mask")

unmasker("Paris is the [MASK] of France.")
```

```python
[
  {'sequence': 'Paris is the capital of France.', 'score': 0.9793, 'token_str': 'capital'},
  {'sequence': 'Paris is the heart of France.',   'score': 0.0051, 'token_str': 'heart'},
  {'sequence': 'Paris is the city of France.',    'score': 0.0027, 'token_str': 'city'},
  {'sequence': 'Paris is the center of France.',  'score': 0.0019, 'token_str': 'center'},
  {'sequence': 'Paris is the centre of France.',  'score': 0.0018, 'token_str': 'centre'}
]
```

> Important: the mask token differs by model family. BERT uses `[MASK]`. RoBERTa and DistilBERT use `<mask>`. Always check the model card to confirm.

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `top_k` | `5` | How many candidate words to return |

```python
unmasker(
    "The scientist discovered a new <mask> that could cure cancer.",
    top_k=3
)
```

### Recommended models

| Model ID | Mask token | Notes |
|---|---|---|
| `bert-base-uncased` | `[MASK]` | Classic, general-purpose |
| `roberta-base` | `<mask>` | Often better than BERT |
| `distilbert-base-uncased` | `[MASK]` | Smaller, faster BERT |
| `allenai/scibert_scivocab_uncased` | `[MASK]` | Trained on scientific papers |

---

## 12. Task 6 — Named Entity Recognition (NER)

**What it does:** Reads text and labels every word or phrase that refers to a real-world entity — people (PER), organizations (ORG), locations (LOC), dates, monetary values, and more. Very useful for information extraction from documents, news articles, or contracts.

**Returns:** `list[dict]` — one dict per detected entity, with keys `entity_group` (label string like `'PER'`), `score` (float), `word` (the matched text), `start` (character index where it begins), and `end` (character index where it ends).

```python
result = ner("Elon Musk founded SpaceX.")
# [
#   {'entity_group': 'PER', 'score': 0.999, 'word': 'Elon Musk', 'start': 0, 'end': 9},
#   {'entity_group': 'ORG', 'score': 0.998, 'word': 'SpaceX',    'start': 18, 'end': 24}
# ]

result[0]["word"]           # 'Elon Musk'
result[0]["entity_group"]   # 'PER'
```

### Basic example

```python
from transformers import pipeline

ner = pipeline("ner", grouped_entities=True)

ner("Elon Musk founded SpaceX in Hawthorne, California in 2002.")
```

```python
[
  {'entity_group': 'PER', 'score': 0.9991, 'word': 'Elon Musk',   'start': 0,  'end': 9},
  {'entity_group': 'ORG', 'score': 0.9985, 'word': 'SpaceX',      'start': 18, 'end': 24},
  {'entity_group': 'LOC', 'score': 0.9978, 'word': 'Hawthorne',   'start': 28, 'end': 37},
  {'entity_group': 'LOC', 'score': 0.9965, 'word': 'California',  'start': 39, 'end': 49},
]
```

### Entity types (standard labels)

| Label | Meaning |
|---|---|
| `PER` | Person |
| `ORG` | Organization, company, institution |
| `LOC` | Location, country, city, geographic feature |
| `MISC` | Miscellaneous — product names, events, nationalities |

> Some models use different label sets (e.g., `B-PER`, `I-PER` in BIO format). `grouped_entities=True` hides this complexity by merging multi-token entities.

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `grouped_entities` | `False` | `True` = merge sub-word tokens that belong to the same entity into one result (e.g. "Hugging" + "Face" → "Hugging Face"). `False` = return every sub-word token as a separate result, which is hard to read |
| `aggregation_strategy` | `"none"` | How to aggregate sub-word tokens. Use `"simple"` or `"first"` as alternatives to `grouped_entities` |
| `stride` | `0` | Overlap between chunks when input is split (helps avoid edge-of-chunk errors) |

```python
# These two calls are equivalent — both merge multi-token entities
ner = pipeline("ner", grouped_entities=True)
ner = pipeline("ner", aggregation_strategy="simple")
```

### Recommended models

| Model ID | Notes |
|---|---|
| `dslim/bert-base-NER` | Default, very solid general-purpose NER |
| `Jean-Baptiste/roberta-large-ner-english` | More accurate, slower |
| `flair/ner-english-large` | Flair-based, excellent accuracy |
| `dslim/bert-base-NER-uncased` | Better when casing is inconsistent |

---

## 13. Task 7 — Question Answering

**What it does:** Given a block of text (the **context**) and a question, the model finds and extracts the exact span of text that answers the question. It does not generate a free-form answer — it only highlights what is already in the context.

This is called **extractive** question answering, as opposed to **generative** QA (which is what ChatGPT does).

**Returns:** `dict` (not a list) — a single dict with keys `answer` (the extracted text string), `score` (float confidence), `start` (character index in context where the answer begins), and `end` (character index where it ends).

```python
result = qa(question="Who founded Tesla?", context="Tesla was founded by Elon Musk.")
# {'answer': 'Elon Musk', 'score': 0.991, 'start': 22, 'end': 31}

result["answer"]   # 'Elon Musk'
result["score"]    # 0.991
```

> If you pass `top_k=3`, the return type changes to `list[dict]` — a list of the top 3 candidate answers, each with the same keys.

### Basic example

```python
from transformers import pipeline

qa = pipeline("question-answering")

qa(
    question="Who founded Microsoft?",
    context="Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975."
)
```

```python
{
  'answer': 'Bill Gates and Paul Allen',
  'score':  0.9822,
  'start':  24,
  'end':    49
}
```

### Passing a long document

For long documents, the pipeline automatically splits the context into overlapping chunks and picks the best answer across all of them:

```python
long_document = """..."""  # Could be thousands of words

qa(
    question="What is the company's main product?",
    context=long_document,
    max_answer_len=50,
    doc_stride=128   # Overlap between chunks (helps avoid cutting an answer in half)
)
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `max_answer_len` | `15` | Maximum number of tokens in the extracted answer |
| `max_seq_len` | `384` | Maximum number of tokens per chunk for long documents |
| `doc_stride` | `128` | Overlap in tokens between consecutive chunks |
| `top_k` | `1` | Return the top K candidate answers instead of just one |
| `handle_impossible_answer` | `False` | `True` = if the model thinks the answer is not in the context, return an empty string `""` instead of guessing. `False` = always return the best span the model found, even if it is wrong |

```python
# Return top 3 possible answers
qa(
    question="What year was the company founded?",
    context=long_document,
    top_k=3
)
```

### Recommended models

| Model ID | Notes |
|---|---|
| `distilbert-base-cased-distilled-squad` | Default, small and fast |
| `deepset/roberta-base-squad2` | Better at "no answer" cases |
| `deepset/deberta-v3-base-squad2` | Very accurate, slower |
| `deepset/minilm-uncased-squad2` | Tiny and fast, good for production |

---

## 14. Task 8 — Summarization

**What it does:** Takes a long piece of text and produces a shorter version that retains the most important information. The model *generates* the summary (it writes new sentences), rather than extracting sentences verbatim from the original. This is called **abstractive** summarization.

**Returns:** `list[dict]` — one dict with a single key `summary_text` (string) containing the generated summary.

```python
result = summarizer(long_article)
# [{'summary_text': 'The Amazon rainforest produces 20% of the world\'s oxygen...'}]

result[0]["summary_text"]   # the summary as a plain string
```

### Basic example

```python
from transformers import pipeline

summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

article = """
    The Amazon rainforest, often referred to as the "lungs of the Earth," produces
    20% of the world's oxygen and is home to 10% of all species on the planet.
    It spans over 5.5 million square kilometers across nine countries. However,
    deforestation has accelerated dramatically over recent decades, driven primarily
    by agricultural expansion, logging, and mining. Scientists warn that if current
    trends continue, the forest could reach a tipping point within decades, after
    which it would begin releasing carbon instead of absorbing it, drastically
    accelerating global warming.
"""

summarizer(article)
```

```python
[{'summary_text': 'The Amazon rainforest produces 20% of the world\'s oxygen and is home to 10% 
   of all species. Scientists warn it could reach a tipping point due to deforestation, 
   after which it would release carbon instead of absorbing it.'}]
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `max_length` | `142` | Maximum tokens in the generated summary |
| `min_length` | `56` | Minimum tokens in the generated summary |
| `do_sample` | `False` | `True` = pick words randomly during generation (adds variety but can hurt coherence). `False` = use beam search for a more controlled, reliable summary. Keep `False` for summarization |
| `num_beams` | `4` | Number of candidate sequences the model explores in parallel. `1` = greedy (fastest, lowest quality). `4` or more = better quality, slower |
| `length_penalty` | `2.0` | `> 1.0` = the model is rewarded for producing longer output (more detailed summary). `< 1.0` = the model is rewarded for being brief. `1.0` = no preference |
| `early_stopping` | `True` | `True` = stop as soon as all beams finish (faster). `False` = keep going until `max_length` is hit |
| `truncation` | `True` | `True` = silently cut the input article if it is too long for the model. `False` = raise an error when the input exceeds the limit |
| `no_repeat_ngram_size` | `3` | `0` = no restriction. `3` = the model cannot repeat any sequence of 3 words that already appeared in the summary |

```python
# Short summary
summarizer(article, max_length=60, min_length=20)

# Longer, more detailed summary
summarizer(article, max_length=200, min_length=100, length_penalty=1.0)
```

### Recommended models

| Model ID | Notes |
|---|---|
| `facebook/bart-large-cnn` | Default, trained on news articles |
| `sshleifer/distilbart-cnn-12-6` | Smaller, faster version |
| `google/pegasus-xsum` | Better for very short, one-sentence summaries |
| `facebook/bart-large-xsum` | Also great for short summaries |
| `allenai/led-base-16384` | Handles very long documents (up to 16,384 tokens) |

---

## 15. Task 9 — Translation

**What it does:** Translates text from one language to another. The quality depends heavily on the specific language pair and the model chosen.

**Returns:** `list[dict]` — one dict with a single key `translation_text` (string) containing the translated output.

```python
result = translator("Bonjour le monde.")
# [{'translation_text': 'Hello world.'}]

result[0]["translation_text"]   # 'Hello world.'
```

### Basic example

```python
from transformers import pipeline

# French → English
translator = pipeline("translation", model="Helsinki-NLP/opus-mt-fr-en")
translator("Le soleil brille et les oiseaux chantent.")
# [{'translation_text': 'The sun is shining and the birds are singing.'}]

# Spanish → English
translator_es = pipeline("translation", model="Helsinki-NLP/opus-mt-es-en")
translator_es("El aprendizaje automático está transformando la industria.")
# [{'translation_text': 'Machine learning is transforming industry.'}]
```

### Finding the right Helsinki-NLP model

The [Helsinki-NLP organization](https://huggingface.co/Helsinki-NLP) has published over 1,000 translation models. Their naming convention is:

```
Helsinki-NLP/opus-mt-{source_language}-{target_language}
```

| Language | Code |
|---|---|
| English | `en` |
| Spanish | `es` |
| French | `fr` |
| German | `de` |
| Chinese | `zh` |
| Arabic | `ar` |
| Portuguese | `pt` |
| Italian | `it` |
| Russian | `ru` |
| Japanese | `jap` |

So for German → English: `Helsinki-NLP/opus-mt-de-en`.

### Using a task-name shortcut (no model needed)

```python
# Built-in language-pair task names
translator = pipeline("translation_en_to_fr")
translator("Hello, how are you?")
# [{'translation_text': 'Bonjour, comment allez-vous?'}]
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `max_length` | `512` | Maximum tokens in the translated output |
| `min_length` | `0` | Minimum tokens in the translated output |
| `num_beams` | `4` | Beam search — higher = better quality |
| `src_lang` | depends on model | Source language (needed for multilingual models like NLLB) |
| `tgt_lang` | depends on model | Target language (needed for multilingual models like NLLB) |

### Multilingual translation with a single model

Instead of one model per language pair, you can use Meta's NLLB model, which supports 200+ languages:

```python
from transformers import pipeline

# One model, 200+ languages
translator = pipeline("translation", model="facebook/nllb-200-distilled-600M")

translator(
    "Aprender idiomas abre muchas puertas.",
    src_lang="spa_Latn",  # Spanish (Latin script)
    tgt_lang="fra_Latn"   # French (Latin script)
)
# [{'translation_text': "Apprendre des langues ouvre beaucoup de portes."}]
```

NLLB language codes are in the format `{language}_{script}`. Find the full list in the [NLLB model card](https://huggingface.co/facebook/nllb-200-distilled-600M).

---

## 16. Task 10 — Feature Extraction

**What it does:** Converts text into a numerical vector (called an **embedding**). The model does not produce a label or a sentence — it produces a list of numbers that mathematically represent the meaning of your text. Similar texts end up with vectors that are close to each other in space.

This is the foundation for:
- **Semantic search** — find documents by meaning, not keywords
- **Clustering** — automatically group similar documents
- **Recommendation systems** — find similar items

**Returns:** `list[list[list[float]]]` — a 3-dimensional nested list of numbers, not a dict. The shape is `[batch_size, num_tokens, hidden_size]`. For a single sentence with a 768-dimensional model you get shape `[1, N, 768]` where N is the number of tokens.

```python
result = extractor("Hello world")
# result is a list → result[0] is the first (and only) sentence
# result[0] is a list of token vectors → result[0][0] is the first token's vector
# result[0][0] is a list of 768 floats

import numpy as np
vector = np.mean(result[0], axis=0)  # average across tokens → shape (768,)
```

### Basic example

```python
from transformers import pipeline
import numpy as np

extractor = pipeline("feature-extraction", model="distilbert-base-uncased")

# Returns a 3D tensor: [batch, tokens, hidden_size]
output = extractor("Machine learning is fascinating.")

# Convert to numpy and take the mean across the token dimension
# to get a single vector representing the whole sentence
vector = np.mean(output[0], axis=0)

print(f"Vector shape: {vector.shape}")  # (768,) — 768-dimensional embedding
print(f"First 5 values: {vector[:5]}")
```

### Sentence-level embeddings (the right way)

For most use cases you want a **sentence embedding** — a single vector for the whole input. The best way to do this is to use a model from the `sentence-transformers` family (they are designed specifically for this):

```python
from transformers import pipeline

# SentenceTransformer models are optimized for semantic similarity
extractor = pipeline(
    "feature-extraction",
    model="sentence-transformers/all-MiniLM-L6-v2",
    pooling="mean"  # Automatically averages token vectors to get one vector per sentence
)

v1 = extractor("The cat sat on the mat.")[0]
v2 = extractor("A kitten rested on the rug.")[0]

# Cosine similarity — close to 1.0 means similar meaning
from sklearn.metrics.pairwise import cosine_similarity
similarity = cosine_similarity([v1], [v2])[0][0]
print(f"Similarity: {similarity:.3f}")  # e.g. 0.843
```

---

## 17. Task 11 — Image Classification

**What it does:** Pipelines are not limited to text! The image classification pipeline takes an image and returns labels describing what is in it.

**Returns:** `list[dict]` — one dict per predicted label, sorted by score. Each dict has keys `label` (string) and `score` (float).

```python
result = classifier("cat.jpg")
# [{'label': 'tabby cat', 'score': 0.524}, {'label': 'tiger cat', 'score': 0.236}]

result[0]["label"]   # most likely label
result[0]["score"]   # its confidence
```

```python
from transformers import pipeline

# Works with a local file path, a URL, or a PIL image
classifier = pipeline("image-classification")

result = classifier("https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/RedCat_8727.jpg/120px-RedCat_8727.jpg")
```

```python
[
  {'label': 'tabby, tabby cat',           'score': 0.5241},
  {'label': 'tiger cat',                  'score': 0.2364},
  {'label': 'Egyptian cat',               'score': 0.1432},
]
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `top_k` | `5` | How many top labels to return |

### Recommended models

| Model ID | Notes |
|---|---|
| `google/vit-base-patch16-224` | Default vision transformer |
| `microsoft/resnet-50` | Classic CNN-based classifier |
| `openai/clip-vit-base-patch32` | CLIP — can match images to text labels |

---

## 18. Task 12 — Automatic Speech Recognition

**What it does:** Converts audio (speech) into text. Useful for transcription, voice commands, and accessibility tools.

**Returns:** `dict` (not a list) — a single dict with a key `text` (string) containing the full transcription. If `return_timestamps=True`, it also includes a `chunks` key with a list of dicts, each containing `text`, `timestamp` start, and `timestamp` end.

```python
result = transcriber("audio.mp3")
# {'text': 'Hello, welcome to the course.'}

result["text"]   # 'Hello, welcome to the course.'

# With timestamps:
result = transcriber("audio.mp3", return_timestamps=True)
# {'text': 'Hello...', 'chunks': [{'text': 'Hello', 'timestamp': (0.0, 0.5)}, ...]}
result["chunks"][0]["timestamp"]   # (0.0, 0.5)  → start=0.0s, end=0.5s
```

```python
from transformers import pipeline

transcriber = pipeline("automatic-speech-recognition", model="openai/whisper-base")

# Pass a local audio file path
result = transcriber("audio.mp3")
print(result["text"])
# "Hello, welcome to the Hugging Face course on Transformers."
```

### Most common parameters

| Parameter | Default | What it does |
|---|---|---|
| `chunk_length_s` | `30` | Audio chunk size in seconds for long files |
| `stride_length_s` | `None` | Overlap between chunks |
| `return_timestamps` | `False` | `True` = also return the start and end time (in seconds) for each transcribed segment, useful for subtitles. `False` = return only the transcribed text |
| `generate_kwargs` | `{}` | Pass generation arguments (e.g., `language`, `task`) to Whisper |

```python
# Transcribe with timestamps + force a specific source language
transcriber(
    "lecture.mp3",
    return_timestamps=True,
    generate_kwargs={"language": "english", "task": "transcribe"}
)
```

### Recommended models

| Model ID | Notes |
|---|---|
| `openai/whisper-tiny` | Fastest, least accurate |
| `openai/whisper-base` | Good balance |
| `openai/whisper-small` | Better accuracy |
| `openai/whisper-large-v3` | Best accuracy, needs GPU |

---

## 19. Can You Use OpenAI, Anthropic, or Gemini with This Library?

**Short answer: No — not through the `pipeline()` function.**

The Hugging Face `transformers` library is built for models whose weights you can download and run locally. OpenAI (GPT-4, GPT-4o), Anthropic (Claude), and Google (Gemini) are **proprietary, closed-source APIs**. Their weights are not publicly available — you can only access them through their own HTTP APIs, not by downloading and running them yourself.

| Provider | Can you load via `pipeline()`? | Why not |
|---|---|---|
| OpenAI (GPT-4, GPT-4o) | ❌ No | Closed API, weights not available |
| Anthropic (Claude) | ❌ No | Closed API, weights not available |
| Google (Gemini) | ❌ No | Closed API, weights not available |
| Meta Llama 3 | ✅ Yes | Open weights on the Hub (requires license) |
| Mistral | ✅ Yes | Open weights on the Hub |
| Google Flan-T5 | ✅ Yes | Open weights on the Hub |

### How to use OpenAI, Anthropic, and Gemini — through their own SDKs

You use them through separate, official Python packages:

**OpenAI (GPT-4, GPT-4o)**
```python
pip install openai
```
```python
from openai import OpenAI

client = OpenAI(api_key="your-key-here")

response = client.chat.completions.create(
    model="gpt-4o",
    messages=[{"role": "user", "content": "Explain transformers in one sentence."}]
)
print(response.choices[0].message.content)
```

**Anthropic (Claude)**
```python
pip install anthropic
```
```python
import anthropic

client = anthropic.Anthropic(api_key="your-key-here")

message = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=256,
    messages=[{"role": "user", "content": "Explain transformers in one sentence."}]
)
print(message.content[0].text)
```

**Google (Gemini)**
```python
pip install google-genai
```
```python
from google import genai

client = genai.Client(api_key="your-key-here")

response = client.models.generate_content(
    model="gemini-2.5-pro",
    contents="Explain transformers in one sentence."
)
print(response.text)
```

### Can I use the Hugging Face Inference API (not local)?

Yes! Hugging Face has its own cloud inference API. You can call it without downloading any model and without a GPU. This is useful for prototyping or for models too large to run locally:

```python
pip install huggingface_hub
```

```python
from huggingface_hub import InferenceClient

client = InferenceClient(
    model="mistralai/Mistral-7B-Instruct-v0.2",
    token="your-hf-token"  # Get this from huggingface.co/settings/tokens
)

# Chat-like interface
response = client.chat_completion(
    messages=[{"role": "user", "content": "What is a transformer in AI?"}],
    max_tokens=200
)
print(response.choices[0].message.content)
```

This calls the model on Hugging Face's servers — you don't need a GPU. The free tier has rate limits; there is a paid tier for production use.

### Using all providers through a unified interface

If you want to switch between OpenAI, Anthropic, Gemini, and Hugging Face models easily — using the same code structure — you can use a wrapper library:

```python
pip install langchain langchain-openai langchain-anthropic
```

```python
from langchain_openai import ChatOpenAI
from langchain_anthropic import ChatAnthropic

# Swap models by changing one line
# model = ChatOpenAI(model="gpt-4o")
model = ChatAnthropic(model="claude-opus-4-5")

response = model.invoke("What is a transformer in AI?")
print(response.content)
```

---

## 20. Key Takeaways

| Concept | Plain-English Explanation |
|---|---|
| `pipeline("task")` | One-line access to a pre-trained model for any task. Handles everything automatically. |
| `model=` argument | Swap the default model for any model from the Hub using its model ID |
| Task name | The string that tells the pipeline what kind of job to do (e.g., `"summarization"`) |
| `device=0` | Run on GPU for much faster inference on large models |
| `device_map="auto"` | Let the library pick the best device automatically |
| The Hugging Face Hub | A public library of hundreds of thousands of free, downloadable models |
| Inference Widget | Live demo on every model page — test any model in your browser without code |
| `do_sample=True` | Enable random sampling for text generation (required for temperature to work) |
| `temperature` | Creativity dial: low = focused/predictable, high = varied/creative |
| `num_beams` | Quality dial for generation/translation/summarization: higher = better but slower |
| OpenAI/Claude/Gemini | Cannot be used via `pipeline()` — use their own SDKs instead |
| HF Inference API | Hugging Face's own cloud API — run big models without a local GPU |
