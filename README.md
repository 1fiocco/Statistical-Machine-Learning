# Dream Continuation Generation with Qwen3

This project investigates whether a language model can generate a plausible continuation from the first part of a dream.

The central hypothesis is that dreams, despite their chaotic and unusual content, contain recurring **emotional, thematic, and narrative patterns**. We therefore test whether dream continuation improves when a model is:

1. fine-tuned specifically on dream narratives;
2. provided with semantic and demographic metadata about the dream.

The final experiment compares four versions of **Qwen3-1.7B-Base**:

| Model | Fine-tuned on dreams | Uses metadata |
|---|---:|---:|
| Base Qwen3 | No | No |
| Base Qwen3 + metadata | No | Yes |
| Fine-tuned Qwen3 | Yes | No |
| Fine-tuned Qwen3 + metadata | Yes | Yes |

---

## Research Questions

The project addresses three main questions:

- Does fine-tuning improve the model's ability to predict the real continuation of a dream?
- Does adding emotion, topic, sex, and age information improve performance?
- Is the combination of fine-tuning and metadata the best configuration?

---

## Dataset

The project uses an enriched version of the **DreamBank** dataset containing:

- **21,000 dream reports**
- approximately **70 dreamers**
- narratives collected across different periods and personal contexts

The final CSV contains six columns:

| Column | Description |
|---|---|
| `dream_text` | Complete dream narrative |
| `dominant_macro_emotion` | Emotion with the highest classification score |
| `second_dominant_macro_emotion` | Emotion with the second-highest score |
| `dream_topic` | Topic assigned through BERTopic |
| `sex` | Available sex information about the dreamer |
| `age` | Available age category of the dreamer |

The dataset contains **13 macro-emotion categories**, including:

- `admiration_approval`
- `joy_amusement`
- `fear_anxiety`
- `anger_frustration`
- `sadness_grief`
- `embarrassment_confusion`
- `curiosity_surprise`
- `disappointment_remorse`

Dreams are also assigned to **25 thematic topics**, representing recurring themes such as driving, examinations, relationships, animals, supernatural events, illness, school, work, and marriage.

### Semantic enrichment

The original dream reports were enriched using two pretrained methods:

- **RoBERTa-based emotion classification**, used to estimate emotion scores and group them into macro-emotions;
- **BERTopic**, used to cluster dream embeddings and assign a thematic topic to each report.

Age and sex information were extracted from the available descriptions of the dreamers.

---

## Personal Dream Dataset

A second dataset containing **41 dreams written by group members** was created to test whether the DreamBank-based pipeline also generalizes to personal dream narratives.

Each personal dream was manually labelled with:

- one main emotion;
- one main topic.

The automatic emotion classifier achieved:

- **44% exact agreement** with the manual emotion;
- **78% agreement at the broader positive/negative valence level**.

The personal dreams were also projected into the DreamBank embedding space. They generally appeared inside the main DreamBank distribution.

A Maximum Mean Discrepancy test produced:

```text
Observed MMD: 0.020
Baseline MMD: 0.0002
p-value: 0.002
