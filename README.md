# 🔍  Text Detective

**Text Detective** is a small Natural Language Processing (NLP) application that investigates any piece of text. Paste in a paragraph and it will clean the text, split it into sentences, count characters and words, break it into tokens, find emails, dates, phone numbers and hashtags, and then use Google's Gemini model to summarise the text, judge its mood, pick out keywords, write a short quiz and answer questions about it.

### ✨ Features

| Station | What it does |
| ----- | -----|
| 🧽 Cleaner | 	Fixes look-alike characters, curly quotes, long dashes, repeated !!! and extra spaces |
| ✂️ Sentence cutter | Splits text into sentences without being fooled by abbreviations or decimals |
| 🔢 Counter | 	Counts characters (with and without spaces), words and sentences |
| 🧩 Token breaker | Splits text into word tokens and compares with Gemini's subword token count |
| 🔎 Pattern finder | Extracts emails, dates, hashtags and phone numbers with regular expressions |
| 🧠 Gemini brain | Summary, mood (positive/negative/neutral), top keywords, quiz questions, and question answering |
| 📚 Librarian | Saves every analysed text to my_library.csv with a unique ID (DOC_0001, …) and a source label |
| 🖥️ App screen | A simple web interface with an Analyse tab and an Ask a question tab |

### 🧰 Requirements

- Google Colab or Jupyter Notebook
- A Gemini API key from Google AI Studio
- Python libraries
   - `google-genai`: talks to the Gemini model
   - `pandas`: tables and CSV files
   - `gradio`: the app screen
   - `re`, `unicodedata`, `os`, `time`, `datetime`: built into Python

### 🚀 Getting started

**1. Open the notebook**
  
   Open `Text_Detective.ipynb`

**2. Add your Gemini API key**

**3. Install Gradio**

Run the install cell:

```python
!pip install -q gradio
```

Red "dependency conflict" warnings after this step are normal and can be ignored.

**4. Run all cells**

Click **Runtime → Run all**. The app appears at the bottom of the notebook.

To get a link you can open on another device or share, change the last line to:

```python
app.launch(share=True)
```

### 🖱️ How to use it

**Analyse tab**

1. Paste any text into the Your **text box**.
2. Click **Investigate!**
3. Read the report: counts, sentences, tokens, patterns found and Gemini's insights.

**Ask a question tab**

1. Keep your text in the Your **text box**.
2. Type a question, for example "When is the site visit?"
3. Click **Ask Gemini**.

Gemini answers only from your text. If the answer isn't there, it replies "The text does not say."

### ⚙️ How it works

The text moves through the stations in order, like a factory conveyor belt:

```text
Raw text
   │
   ▼
🧽 clean_text()        →  tidy text
   ▼
✂️ split_sentences()   →  list of sentences
   ▼
🔢 count_stats()       →  characters, words, sentences
   ▼
🧩 word_tokens()       →  word tokens  (+ gemini_token_count())
   ▼
🔎 find_patterns()     →  emails, dates, hashtags, phones
   ▼
🧠 gemini_insights()   →  summary, mood, keywords, quiz
   ▼
📚 save_to_library()   →  new row in my_library.csv
   ▼
📄 Report shown on the app screen
```

All of this is run by one function, `analyse(text)`. The question tab uses `ask_about_text(text, question)`.

**Main functions**

| Function | Purpose |
|---|---|
|`ask_gemini(prompt)` |	Sends a prompt to Gemini; retries automatically if Gemini is busy (error 503) |
|`clean_text(text)` | 	Standardises characters, punctuation and whitespace|
|`split_sentences(text)` |Rule-based sentence splitting with abbreviation and decimal protection |
|`count_stats(text, sentences)` |Character, word and sentence counts |
|`word_tokens(text)` | 	Word-level tokenization (keeps contractions like don't)|
|`gemini_token_count(text)` | 	Gemini's own subword token count |
|`find_patterns(text)` | 	Regex extraction of emails, dates, hashtags and phones |
|`gemini_insights(text)` | Summary, mood, keywords and quiz from Gemini |
|`ask_about_text(text, question)` | Answers a question using only the given text |
|`save_to_library(text, source, stats)` | Saves the text with a unique ID and source label |
|`analyse(text)` |	Runs every station and builds the report |

### 📁 Files

|File | Description|
|---|---|
|`Text_Detective.ipynb` |	The Colab notebook containing the whole application |
| `my_library.csv` | Created automatically; stores every analysed text |

### ⚠️ Known limitations

- **Cleaning keeps capital letters on purpose**. The sentence cutter and Gemini need them, so this cleaner does not lowercase.
- **The sentence cutter uses a fixed abbreviation list**. Abbreviations not on the list (e.g. Capt.) may cause wrong splits, and a sentence ending with an abbreviation (...at 5 p.m. Then...) may not be split.
- **Regex checks shape, not meaning**. A date like 30 February 2026 will still be accepted, and 01/12/2026 could mean 1 December or 12 January.
- **Phone patterns** expect numbers starting with +, 0 or an area code in brackets.
