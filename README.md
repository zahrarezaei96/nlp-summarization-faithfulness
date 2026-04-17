# Faithfulness Evaluation of Neural Summarization Models
### A Comparative Study of BART, PEGASUS, and Flan-T5

**Course:** Natural Language Processing 1038141 — Sapienza University of Rome  
**Task:** Automatic Summarization  
**Type:** Homework Project (HWp)

---

## Research Question

> Do different neural summarization models differ significantly in their faithfulness to the source text, and does this vary across domains and document lengths?

---

## Models

| Model | Type | HuggingFace |
|-------|------|-------------|
| BART | Encoder-Decoder (fine-tuned on CNN/DM) | [facebook/bart-large-cnn](https://huggingface.co/facebook/bart-large-cnn) |
| PEGASUS | Encoder-Decoder (fine-tuned on XSum) | [google/pegasus-xsum](https://huggingface.co/google/pegasus-xsum) |
| Flan-T5 | Instruction-tuned LLM (zero-shot) | [google/flan-t5-base](https://huggingface.co/google/flan-t5-base) |

---

## Datasets

| Dataset | Size (eval) | Source |
|---------|-------------|--------|
| CNN/DailyMail | 100 test examples | [abisee/cnn_dailymail](https://huggingface.co/datasets/abisee/cnn_dailymail) |
| XSum | 100 test examples | [EdinburghNLP/xsum](https://huggingface.co/datasets/EdinburghNLP/xsum) |

---

## Metrics

- **ROUGE-1, ROUGE-2, ROUGE-L** — lexical overlap with reference summaries
- **SummaC** — faithfulness / factual consistency score (NLI-based)
- **Wilcoxon signed-rank test** — statistical significance of model differences

---

## Main Results

| Model / Dataset | ROUGE-1 | ROUGE-2 | ROUGE-L | SummaC |
|-----------------|---------|---------|---------|--------|
| BART (CNN/DM) | 0.3424 | 0.1477 | 0.2556 | **+0.2508** |
| BART (XSum) | 0.2103 | 0.0365 | 0.1362 | **+0.4392** |
| PEGASUS (CNN/DM) | 0.2744 | 0.0948 | 0.1978 | −0.2239 |
| PEGASUS (XSum) | **0.4629** | **0.2463** | **0.3882** | −0.3049 |
| Flan-T5 (CNN/DM) | 0.2497 | 0.0865 | 0.1927 | −0.1964 |
| Flan-T5 (XSum) | 0.3400 | 0.1025 | 0.2554 | −0.3655 |

**Key finding:** Higher ROUGE does not imply higher faithfulness. PEGASUS achieves the best ROUGE on XSum but the worst SummaC score.

---

## How to Run

1. Open `nlp_hwp_v2.ipynb` in [Google Colab](https://colab.research.google.com/)
2. Set runtime to **GPU**: `Runtime → Change runtime type → GPU`
3. Run all cells in order (Step 1 installs dependencies and requires a runtime restart)
4. Results are automatically saved to your Google Drive under `NLP_HWp_Results/`

**Requirements** (installed automatically in Step 1):
```
datasets==2.14.0
huggingface_hub==0.16.4
transformers==4.35.0
fsspec==2023.6.0
rouge-score
sentencepiece
summac
scipy
```

---

## Repository Structure

```
├── nlp_hwp_v2.ipynb          # Main experiment notebook
├── results/
│   ├── results_main.csv       # ROUGE + SummaC for all models/datasets
│   ├── results_by_length.csv  # SummaC broken down by document length
│   ├── results_statistical.csv # Wilcoxon test results
│   └── qualitative_examples.json # 10 annotated examples with scores
└── README.md
```

---

## References

- Lewis et al. (2020). BART. ACL. https://arxiv.org/abs/1910.13461
- Zhang et al. (2020). PEGASUS. ICML. https://arxiv.org/abs/1912.08777
- Laban et al. (2022). SummaC. TACL. https://github.com/tingofurro/summac
- Narayan et al. (2018). XSum. EMNLP.
- Hermann et al. (2015). CNN/DailyMail. NeurIPS.
