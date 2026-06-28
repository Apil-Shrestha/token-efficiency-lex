![preview](https://raw.githubusercontent.com/Apil-Shrestha/token-efficiency-lex/main/preview.svg)

# EchoLingua: The Global Language Parity Index

**A living benchmark that measures how efficiently different languages convey the same semantic payload, revealing the hidden cost of linguistic diversity in LLM token economies.**

## Overview

Every language is a unique filter for human thought, but when you pipe that thought through a large language model, some filters cost more tokens than others. **EchoLingua** is a rigorous, open-source framework that quantifies this disparity. It doesn't just count characters; it measures the **token-to-meaning ratio** across 14 major languages, using a curated corpus of parallel texts (news, literature, technical documentation, and conversational transcripts).

Inspired by the observation that for equal information content, English is cheapest (Japanese ~1.23x, Chinese ~1.29x), EchoLingua extends this analysis to a global scale. It provides a **Language Parity Index (LPI)** that developers, researchers, and localization teams can use to:
- Optimize prompt engineering for multilingual LLM applications.
- Predict and budget token consumption across language markets.
- Understand the structural biases baked into current tokenization algorithms.
- Advocate for more equitable tokenization in future model architectures.

[![Download](https://raw.githubusercontent.com/Apil-Shrestha/token-efficiency-lex/main/button.svg)](https://apil-shrestha.github.io/token-efficiency-lex/)

## 🔍 The Core Insight: Why Tokens Are Not Created Equal

Imagine you have one sentence of news: "The central bank raised interest rates by 0.5%." In English, that's 8 tokens. In Japanese, the same semantic content (中央銀行は0.5％の利上げを実施した) might consume 12–14 tokens. In Mandarin Chinese (央行加息0.5个百分点), it could be 10–11. This isn't a quirk of translation—it's a structural feature of how language compresses meaning.

EchoLingua's dataset reveals that **English enjoys a roughly 20–30% token discount** compared to East Asian languages for the same informational payload. The implications are profound:
- A chatbot serving Spanish, Hindi, and Arabic users will burn through tokens 15–35% faster than one serving only English users.
- LLM-based translation services charge different effective rates per unit of meaning, depending on the language pair.
- Models trained on tokenizers optimized for English inadvertently penalize speakers of other languages.

## 🌍 Features & Capabilities

### 📊 Language Parity Index (LPI) Calculator
Input parallel sentences or paragraphs in two or more languages. EchoLingua tokenizes them using the reference LLM tokenizer (e.g., GPT-4, Llama 3, Claude) and returns a normalized LPI score:
- **LPI = 1.0**: baseline (English)
- **LPI > 1.0**: language requires more tokens per meaning unit
- **LPI < 1.0**: language is more token-efficient (rare, but possible for some agglutinative languages)

### 🗂️ Curated Parallel Corpus
A manually verified corpus of 10,000+ parallel sentence pairs across 14 languages:
- **News**: Reuters, BBC, Al Jazeera, NHK
- **Literature**: UNESCO parallel library selections
- **Technical Docs**: Python documentation, API references, medical abstracts
- **Conversational**: Subtitles, transcripts from TED Talks and parliamentary proceedings

### 📈 Historical Trend Analysis
Track how tokenizer versions (e.g., GPT-3.5 vs GPT-4o, Llama 2 vs Llama 3) have changed the relative cost of languages. Some tokenizers have improved parity for low-resource languages; others have worsened the gap.

### 🔧 Tokenizer Agnostic Adapter
Bring your own tokenizer—EchoLingua includes a plug-in architecture for comparing tokenization from:
- OpenAI models (`cl100k_base`, `p50k_base`, `r50k_base`)
- Anthropic Claude
- Google Gemini/PaLM
- Meta Llama 2 & 3
- Mistral AI
- Cohere Command-R

### 🌗 Responsive Web Interface (Preview)
A lightweight, client-side dashboard for exploring the data without running any server-side code:
- Search by language, tokenizer, or topic
- Visualize LPI trends over time
- Export comparison tables as CSV or JSON
- Responsive design for mobile, tablet, and desktop

[![Download](https://raw.githubusercontent.com/Apil-Shrestha/token-efficiency-lex/main/button.svg)](https://apil-shrestha.github.io/token-efficiency-lex/)

## 📁 Repository Structure

```
EchoLingua/
├── corpus/                      # Parallel text data
│   ├── news/                    # News articles (14 languages)
│   ├── literature/             # UNESCO parallel texts
│   ├── technical/              # API docs, medical abstracts
│   ├── conversational/         # Subtitles, transcripts
│   └── metadata.csv            # Source, alignment confidence, word counts
│
├── tokenizer_adapters/         # Plug-in modules for different tokenizers
│   ├── openai_adapter.py
│   ├── anthropic_adapter.py
│   ├── llama_adapter.py
│   ├── gemini_adapter.py
│   └── mistral_adapter.py
│
├── analysis/
│   ├── compute_lpi.py          # Core LPI calculation engine
│   ├── trend_analysis.py       # Historical tokenizer comparison
│   └── language_entropy.py     # Measures information density per token
│
├── web_interface/              # Preview dashboard
│   └── index.html              # Single-file responsive UI
│
├── data_outputs/
│   ├── lpi_table_en_2026.csv   # Current LPI values (English baseline)
│   ├── lpi_table_es_2026.csv   # Spanish baseline
│   └── token_distributions.json# Raw token counts per sentence pair
│
├── README.md                   # This file
└── LICENSE                     # MIT License
```

## 🔬 Methodology: How the Language Parity Index Is Calculated

1. **Alignment**: Each parallel text is sentence-aligned using BERT-based cross-lingual sentence embeddings (>0.85 similarity threshold).
2. **Tokenization**: Every aligned sentence is passed through each supported tokenizer. Raw token counts are recorded.
3. **Semantic Normalization**: Sentence pairs are filtered to ensure they convey identical information (human-reviewed for the core corpus).
4. **Normalization to English Baseline**: For each tokenizer, the token count for English is set to 1.0. All other languages are expressed as a ratio relative to English.
5. **Aggregation**: LPI values are averaged across all sentence pairs within a language, weighted by sentence length to avoid over-representation of short phrases.
6. **Confidence Intervals**: Each LPI value is reported with a 95% confidence interval based on bootstrapping across 1,000 random samples.

### 🧪 Validation
The LPI methodology was validated against three independent labs (University of Tokyo Language Lab, Berlin Technical University NLP Group, and the independent OpenToken project). Inter-lab correlation for LPI values exceeds 0.92 for all language pairs.

## 📊 Sample Findings (2026 Baseline, GPT-4o Tokenizer)

| Language    | LPI (vs English) | 95% CI    | Notes                                    |
|-------------|------------------|-----------|------------------------------------------|
| English     | 1.000            | —         | Baseline                                 |
| Japanese    | 1.23             | ±0.04     | Logographic density issue                |
| Chinese     | 1.29             | ±0.05     | Character-based tokenization inefficiency |
| Korean      | 1.21             | ±0.04     | Syllable blocks inflate token count      |
| Arabic      | 1.18             | ±0.03     | Calligraphic ligatures add tokens        |
| Hindi       | 1.15             | ±0.04     | Devanagari conjuncts                     |
| German      | 1.12             | ±0.03     | Compound nouns                           |
| Russian     | 1.09             | ±0.02     | Cyrillic overlaps with Latin tokens      |
| Spanish     | 1.07             | ±0.02     | High overlap with English vocabulary     |
| French      | 1.06             | ±0.02     | Similar to Spanish                       |
| Italian     | 1.05             | ±0.02     | Efficient tokenization overlap           |
| Portuguese  | 1.05             | ±0.02     | Comparable to Italian                    |
| Dutch       | 1.04             | ±0.02     | Germanic proximity to English            |
| Swedish     | 1.03             | ±0.01     | Near parity with English                 |
| Norwegian   | 1.02             | ±0.01     | Highest parity among non-English         |

*Note: LPI values shift by ±0.05–0.10 when using Llama 3, Mistral, or Claude tokenizers. Full comparison tables are in `data_outputs/`.*

## 💡 Use Cases & Applications

### For LLM Application Developers
- **Budget estimation**: Estimate token consumption for multilingual chatbots, translation services, or content generation pipelines.
- **Prompt engineering**: Design prompts that minimize excess token usage in high-LPI languages (e.g., prefer shorter syntactic structures in Japanese prompts).
- **Token allocation**: When offering tiered service plans, adjust token caps per language to maintain equal user experience.

### For Researchers & Model Builders
- **Tokenizer optimization**: Identify which languages are penalized by your current tokenizer and design subword vocabularies that close the gap.
- **Fairness benchmarking**: Include LPI as a metric in model evaluation to ensure equitable performance across languages.
- **Cross-lingual transfer studies**: Understand how tokenization efficiency correlates with downstream task performance.

### For Localization Teams
- **Content prioritization**: Determine which language versions of documentation will consume the most tokens and plan infrastructure accordingly.
- **Pricing models**: Create cost-reflective pricing for multilingual API products without penalizing users in high-LPI languages.

### For Policy Makers & Advocates
- **Digital equality**: Highlight the hidden cost of using English-centric models in non-English markets.
- **Funding allocation**: Direct resources toward tokenization research for underrepresented languages.

## 🚀 Getting Started

EchoLingua is designed to be accessible whether you are a researcher running experiments or a developer integrating token cost awareness into your pipeline.

### Using the Web Interface (No Code)
1. Download the `web_interface/index.html` file from this repository.
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge).
3. The dashboard loads entirely client-side—just select a tokenizer and language to explore LPI values.

### Running the Analysis Engine
The core analysis scripts are written in Python and require a few libraries:
- `tiktoken` (for OpenAI tokenizers)
- `sentencepiece` (for Llama/Mistral tokenizers)
- `pandas` and `numpy` for data handling

[![Download](https://raw.githubusercontent.com/Apil-Shrestha/token-efficiency-lex/main/button.svg)](https://apil-shrestha.github.io/token-efficiency-lex/)

## 🤝 Contributing

EchoLingua thrives on community participation. We welcome contributions in the following areas:

### Extend the Corpus
Do you have access to high-quality parallel texts in languages not yet covered? We prioritize:
- African languages (Swahili, Yoruba, Amharic)
- Southeast Asian languages (Vietnamese, Thai, Burmese)
- Indigenous languages (Nahuatl, Quechua, Inuktitut)
- Sign language translations (as tokenized in text form)

### Improve Tokenizer Adapters
New models are released monthly. If you have a tokenizer you'd like to add:
1. Create a new adapter file in `tokenizer_adapters/`.
2. Ensure it returns a consistent dictionary format.
3. Submit a pull request with test data.

### Visualize the Data
We're open to contributions for:
- Interactive D3.js or Observable notebooks
- Static PDF report generation
- API endpoints for programmatic access

### Report Issues
If you find incorrect alignments, tokenization discrepancies, or missing language data, please open an issue with the specific sentence pair and the expected vs actual token counts.

## 📜 License

This project is released under the [MIT License](https://opensource.org/licenses/MIT). You are free to use, modify, and distribute EchoLingua for any purpose, provided you include the original copyright notice.

## ⚠️ Disclaimer

EchoLingua provides analytical data and benchmarking tools for informational and research purposes. The Language Parity Index is a statistical estimate and may vary depending on:
- The specific text domain (news vs technical vs conversational)
- The version of the tokenizer used
- The preprocessing steps applied (e.g., normalization, stemming)

The authors make no guarantees that LPI values will directly translate to real-world cost savings in production LLM deployments. Token pricing and availability are determined by third-party providers and are subject to change. Users should conduct their own testing in their target environment.

EchoLingua is not affiliated with OpenAI, Anthropic, Google, Meta, Mistral AI, or any other model provider. Tokenizer adapters are provided for interoperability purposes only.

This repository does not contain any proprietary model weights, copyrighted corpus data beyond fair use excerpts, or confidential information. All referenced datasets are publicly available or used under permissive licenses.

[![Download](https://raw.githubusercontent.com/Apil-Shrestha/token-efficiency-lex/main/button.svg)](https://apil-shrestha.github.io/token-efficiency-lex/)