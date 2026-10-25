# Data-Efficient Large Language Model Training: A Survey

Literature catalog and search-protocol documentation accompanying the survey. The paper catalog follows the manuscript’s study classifications and bibliographic numbering; the search protocol is documented separately below to make the methodology auditable.

> **Scope and reporting status.** The catalog below has been reconciled to the **112 distinct studies** described in the supplied manuscript (18 pre-training, 94 fine-tuning). The database retrieval totals are transcribed from the manuscript.

## Contents

- [Study statistics](#study-statistics)
- [Literature search protocol (Section IV-B)](#literature-search-protocol-section-iv-b)
- [Database-specific search settings](#database-specific-search-settings)
- [Screening, exclusion and reviewer agreement](#screening-exclusion-and-reviewer-agreement)
- [Surrogate-based methods](#surrogate-based-methods)
- [Surrogate-free methods](#surrogate-free-methods)
- [Benchmark and empirical studies](#benchmark-and-empirical-studies)
- [Catalog reconciliation notes](#catalog-reconciliation-notes)

## Study statistics

| Category | Pre-training | Fine-tuning | Total |
|:--|--:|--:|--:|
| Surrogate-based methods | 17 | 59 | **76** |
| Surrogate-free methods | 1 | 26 | **27** |
| Benchmarks and empirical studies | 0 | 9 | **9** |
| **Total distinct studies** | **18** | **94** | **112** |

Classification follows Tables I and IV of the supplied manuscript. Reference **[92]** and reference **[125]** point to different titles/versions of the same arXiv work (2503.15573), so it is counted once here under **[125]**. For rigorous cross-referencing, the manuscript bibliography and Table IV should eventually be deduplicated accordingly.

## Literature search protocol (Section IV-B)

### Search scope and keywords

- **Literature coverage window reported by the manuscript:** 2020-04-17 through 2025-08-07, inclusive. This is **not** the database search execution date.
- **Sources:** Google Scholar, IEEE Xplore, ACM Digital Library, arXiv, and DBLP.
- **Search execution dates:** not documented in the supplied PDF; record the actual date (YYYY-MM-DD) for each source after checking the original search logs or rerunning the search.
- **Search structure:** three keyword groups described by the original study-identification figure (efficiency-related × training-related × model-related). The following concrete terms follow the manuscript concepts and the working Section IV-B query reconstruction.

| Group | Search terms |
|:--|:--|
| **E — efficiency / selection (9)** | `active learning`; `data selection`; `influence function`; `data sampling`; `data filtering`; `fewer data`; `label-efficient`; `data-efficient`; `annotation efficient` |
| **T — training (5)** | `pre-training`; `pretraining`; `fine-tuning`; `instruction tuning`; `alignment` |
| **M — model (5)** | `language model`; `large language model`; `LLM`; `pretrained language model`; `foundation model` |

**Conceptual Boolean query:** `(active learning OR data selection OR influence function OR data sampling OR data filtering OR fewer data OR label-efficient OR data-efficient OR annotation efficient) AND (pre-training OR pretraining OR fine-tuning OR instruction tuning OR alignment) AND (language model OR large language model OR LLM OR pretrained language model OR foundation model)`. Each multiword item should be quoted or grouped according to database syntax.

<details>
<summary><strong>Expanded common Boolean expression (copyable)</strong></summary>

```text
("active learning" OR "data selection" OR "influence function" OR "data sampling" OR "data filtering" OR "fewer data" OR "label-efficient" OR "data-efficient" OR "annotation efficient") AND ("pre-training" OR "pretraining" OR "fine-tuning" OR "instruction tuning" OR "alignment") AND ("language model" OR "large language model" OR "LLM" OR "pretrained language model" OR "foundation model")
```

</details>

**Query provenance:** The submitted manuscript explicitly shows a narrower query beginning with `"language model" AND (...)`, while its study-identification figure reports 9/5/5 keyword groups. The expression above implements the three-group interpretation used in the earlier proposed protocol; it must be compared with original search logs or used for a newly executed reproducibility run. The slash expression `subset/data selection` in the PDF is not an established database operator; use the supported search terms `data selection` and, where historically applicable, `subset selection` as explicit alternatives.

## Database-specific search settings

The table below transfers the draft search-protocol table out of the manuscript. **Reported hits are historical figures printed in the survey**, not results newly obtained with the reconstructed queries; the values cannot be asserted to arise from these exact templates without rerunning or confirming the logs.

| Database | Reported initial hits | Query syntax / fields for a repeatable run | Actual run date |
|:--|--:|:--|:--|
| Google Scholar | 17,500 | Execute one search for every `(m, t, e) ∈ M × T × E` using quoted multiword phrases and space-separated terms (225 combinations). Merge and deduplicate; record the number of results actually examined per query and the year filter. | **TO VERIFY** |
| IEEE Xplore | 3,371 | **Command Search / bibliographic metadata**: enter the expanded common Boolean expression (`OR` within each group, `AND` between groups); restrict the publication-date range to the coverage window. | **TO VERIFY** |
| ACM Digital Library | 4,484 | **Advanced Search**: apply the common Boolean expression to the selected metadata fields (e.g., title, abstract, keywords) using the editable query syntax; record field scope and date filters. | **TO VERIFY** |
| arXiv | 1,758 | **API / Advanced Search**: prefix each quoted term with `all:`; combine with `AND`, `OR`, and `submittedDate:[202004170000 TO 202508072359]` (UTC). Filter formal publication dates separately where applicable. | **TO VERIFY** |
| DBLP | 522 | **Publication Search**: run each `(m, t, e)` as a space-separated word query (225 combinations), union results, apply publication-year/date screening to the bibliographic records. DBLP does not guarantee exact phrase matching in its default interface. | **TO VERIFY** |
| **Total prior to cross-source deduplication** | **27,635** | Sum of the five database retrieval counts reported in the original manuscript. | — |

**Execution notes.** For Google Scholar and DBLP, the `225` queries are the Cartesian product of the 5 model terms, 5 training terms and 9 efficiency terms; they are **proposed executable query templates, not recovered logs**. For systems with query-length limits, execute compatible subqueries and combine the exported results. Record sorting, retrieval depth, all search dates, query text as actually submitted, document fields, filters, and each database export to enable a verifiable replication.

<details>
<summary><strong>Exact query-construction rules and examples</strong></summary>

**Google Scholar (225 query strings, generated without manually listing all combinations)**

```python
from itertools import product
E = ['active learning', 'data selection', 'influence function', 'data sampling', 'data filtering', 'fewer data', 'label-efficient', 'data-efficient', 'annotation efficient']
T = ['pre-training', 'pretraining', 'fine-tuning', 'instruction tuning', 'alignment']
M = ['language model', 'large language model', 'LLM', 'pretrained language model', 'foundation model']
for m, t, e in product(M, T, E):
    print(f'"{m}" "{t}" "{e}"')
```

Example: `"language model" "pre-training" "active learning"`. Search-year restrictions require manual verification for the exact April/August cutoff dates.

**IEEE Xplore / ACM DL**

```text
("active learning" OR "data selection" OR "influence function" OR "data sampling" OR "data filtering" OR "fewer data" OR "label-efficient" OR "data-efficient" OR "annotation efficient") AND ("pre-training" OR "pretraining" OR "fine-tuning" OR "instruction tuning" OR "alignment") AND ("language model" OR "large language model" OR "LLM" OR "pretrained language model" OR "foundation model")
```

**arXiv `search_query` parameter**

```python
from urllib.parse import quote
group = lambda terms: "(" + " OR ".join(
    f'all:"{term}"' for term in terms) + ")"
arxiv_query = " AND ".join([group(E), group(T), group(M),
    "submittedDate:[202004170000 TO 202508072359]"])
print(arxiv_query)
print("https://export.arxiv.org/api/query?search_query=" + quote(arxiv_query, safe=""))
```

The `submittedDate` restriction uses submission timestamps rather than subsequent peer-reviewed publication dates.

**DBLP (225 bibliographic search strings)**

```python
from itertools import product
for m, t, e in product(M, T, E):
    print(f"{m} {t} {e}")
```

Example: `language model pre-training data selection`. In DBLP, spaces mean Boolean AND across words; the default search uses prefixes rather than true quoted-phrase matching. Use its native interface and record any query reformulations.

</details>

**Operator documentation:** [IEEE Xplore](https://ieeexplore.ieee.org/Xplorehelp/searching-ieee-xplore/search-tips/), [ACM Digital Library](https://library-guides.ucl.ac.uk/ACM-Digital-Library/editing-searches), [arXiv API](https://github.com/arXiv/arxiv-docs/blob/develop/source/help/api/user-manual.md), [DBLP Search FAQ](https://dblp.org/faq/1474589.html).

## Screening, exclusion and reviewer agreement

### Study-selection pipeline (reported values)

| Step | Remaining or retrieved records |
|:--|--:|
| Five-database keyword retrieval (sum before deduplication) | 27,635 |
| Title screening | 2,251 |
| Venue screening | 1,163 |
| Abstract screening | 297 |
| Exclude manuscripts shorter than eight pages | 251 |
| Cross-source deduplication | 199 |
| Full-text inclusion screening | 103 |
| Forward snowballing: citation candidates examined | 5,387 |
| Backward snowballing: reference candidates examined | 1,368 |
| Additional relevant papers reported from snowballing | 13 |
| Final distinct studies claimed in the manuscript | 112 |

**Count reconciliation needed:** The manuscript reports 103 papers after full-text screening and 13 *additional relevant* papers found by snowballing, but `103 + 13 = 116`, whereas the manuscript and the reconciled catalog report 112. Until the screening log is checked, these values must not be presented as an internally consistent flow. Verify whether four candidates were duplicate/superseded or subsequently excluded, then update the manuscript flow diagram and this table together.

### Inclusion and exclusion criteria

A paper was eligible when it (i) proposes a technique for data-efficient language-model construction, (ii) empirically analyzes such techniques, or (iii) introduces evaluation benchmarks, datasets or criteria directly relevant to data-efficient LLM construction, as specified in Section IV-A of the manuscript.

Assign a primary reason to each full-text exclusion, using the following codebook:

| Code | Exclusion reason |
|:--|:--|
| X1 | Outside the scope of **data-efficient LLM training**, including non-language modalities outside the declared review scope. |
| X2 | Improves parameter, inference, or systems efficiency **without selecting/curating training data**. |
| X3 | Duplicate or superseded version of an already considered study (prefer formally published versions when applicable). |
| X4 | Does not satisfy the inclusion criteria specified in Section IV-A after full-text examination. |
| X5 | Shorter than eight pages (applied at the reported page-length screening stage). |

**Eight-page threshold.** Following prior systematic literature reviews, including Hou *et al.* (ACM TOSEM 2024; Ref. [24] in the manuscript), the review excluded papers shorter than eight pages as a pragmatic preference for full-length methodological and experimental reports. It is **not a direct measure of paper quality** and can introduce publication-format bias.

### Reviewer screening and agreement

The supplied manuscript reports a **five-person taxonomy-classification team**: three researchers independently assigned categories and two senior researchers adjudicated disagreements (Threats to Validity). This does **not by itself establish** that all five independently screened every candidate paper for inclusion. The precise allocation of screening duties and recorded inter-rater decisions must be checked against the study-selection log.

If all five independently label the same full-text set as `Include` or `Exclude` before discussion, calculate **Fleiss' kappa** using the per-paper category counts `n_ij`:

```text
N = number of papers, n = 5 independent raters, k = 2 labels
P_i   = (sum_j n_ij^2 - n) / (n * (n - 1))
P_bar = (1/N) * sum_i P_i
p_j   = (1/(N*n)) * sum_i n_ij
P_e   = sum_j p_j^2
kappa = (P_bar - P_e) / (1 - P_e)
```

Reference: Fleiss, J. L. (1971), *Measuring nominal scale agreement among many raters*, Psychological Bulletin, 76(5), 378–382, [doi:10.1037/h0031619](https://doi.org/10.1037/h0031619). **No kappa value is asserted here** without original independent decisions; consensus decisions must be recorded separately. If papers were independently assessed by only two reviewers, use the appropriate two-rater agreement measure and report the actual process instead.

### Snowballing

Forward snowballing examines papers citing the included studies (5,387 candidate records); backward snowballing examines references in the included studies (1,368 candidate records). Candidate records are screened against the same eligibility rules; the number reported as 13 additional relevant papers must be reconciled against the unique final study count as noted above.

## Surrogate-based methods

**Definition:** Data-selection methods using an auxiliary evaluator distinct from the target model’s active training loop. An auxiliary evaluator may be a separate scoring model, a small model from the same family, or an evaluation checkpoint, according to Section V of the manuscript.

### Pre-training (17 studies)

- **[28]** CoLoR-Filter: Conditional Loss Reduction Filtering for Targeted Language Model Pre-training (NeurIPS 2024)
- **[42]** Combatting Dimensional Collapse in LLM Pre-Training Data via Diversified File Selection (ICLR 2025)
- **[37]** D2 Pruning: Message Passing for Balancing Diversity and Difficulty in Data Pruning (arXiv 2023)
- **[43]** Data Selection via Optimal Control for Language Models (ICLR 2025)
- **[48]** Decoding Data Quality via Synthetic Corruptions: Embedding-guided Pruning of Code Data (NeurIPS 2023)
- **[45]** FIRE: Flexible Integration of Data Quality Ratings for Effective Pre-Training (arXiv 2025)
- **[49]** Generative Deduplication for Social Media Data Selection (arXiv 2024)
- **[30]** GPT-4o as the Gold Standard: A Scalable and General Purpose Approach to Filter Language Model Pretraining Data (arXiv 2024)
- **[50]** How to Train Data-Efficient LLMs (CoRR 2024)
- **[47]** MASS: MAthematical Data Selection via Skill Graphs for Pretraining Large Language Models (ICML 2025)
- **[39]** MATES: Model-Aware Data Selection for Efficient Pretraining with Data Influence Models (NeurIPS 2024)
- **[46]** Meta-rater: A Multi-dimensional Data Selection Method for Pre-training Language Models (ACL 2025)
- **[41]** MINIPLM: KNOWLEDGE DISTILLATION FOR PRE-TRAINING LANGUAGE MODELS (ICLR 2025)
- **[51]** Predictive Data Selection: The Data That Predicts Is the Data That Teaches (ICML 2025)
- **[44]** QuRating: Selecting High-Quality Data for Training Language Models (ICML 2024)
- **[52]** Rule-based Data Selection for Large Language Models (arXiv 2024)
- **[40]** ToReMi: Topic-Aware Data Reweighting for Dynamic Pre-Training Data Selection (arXiv 2025)

### Fine-tuning (59 studies)

- **[130]** #InsTag: Instruction Tagging for Analyzing Supervised Fine-tuning of Large Language Models (NeurIPS 2023)
- **[78]** A Bayesian Approach to Data Point Selection (NeurIPS 2024)
- **[85]** Active Learning for Effectively Fine-Tuning Transfer Learning to Downstream Task (ACM TIST 2021)
- **[84]** Active Sentence Learning by Adversarial Uncertainty Sampling in Discrete Space (EMNLP 2020)
- **[95]** Aligning Large Language Models to Follow Instructions and Hallucinate Less via Effective Data Filtering (CoRR 2025)
- **[34]** AlpaGasus: Training A Better Alpaca with Fewer Data (arXiv 2023)
- **[89]** Can Data Diversity Enhance Learning Generalization? (ACL 2022)
- **[96]** Clustering and Ranking: Diversity-preserved Instruction Selection through Expert-aligned Quality Estimation (EMNLP 2024)
- **[97]** CodeACT: Code Adaptive Compute-efficient Tuning Framework for Code LLMs (arXiv 2024)
- **[93]** Cold-Start Data Selection for Few-shot Language Model Fine-tuning: A Prompt-Based Uncertainty Propagation Approach (ACL 2023)
- **[98]** CrowdSelect: Synthetic Instruction Data Selection with Multi-LLM Wisdom (arXiv 2025)
- **[36]** D3: Diversity, Difficulty, and Dependability-Aware Data Selection for Sample-Efficient LLM Instruction Tuning (IJCAI 2025)
- **[99]** Data Diversity Matters for Robust Instruction Tuning (EMNLP 2024)
- **[79]** Data Valuation using Neural Networks for Efficient Instruction Fine-Tuning (arXiv 2025)
- **[129]** Data-efficient Fine-tuning for LLM-based Recommendation (SIGIR 2024)
- **[35]** DavIR: Data Selection via Implicit Reward for Large Language Models (ACL 2025)
- **[86]** DEFT-UCS: Data Efficient Fine-Tuning for Pre-Trained Language Models via Unsupervised Core-Set Selection for Text-Editing (EMNLP 2024)
- **[87]** Diversify and Conquer: Diversity-Centric Data Selection with Iterative Refinement (arXiv 2024)
- **[32]** Diversity as a Reward: Fine-Tuning LLMs on a Mixture of Domain-Undetermined Data (arXiv 2025)
- **[33]** Diversity Measurement and Subset Selection for Instruction Tuning Datasets (arXiv 2024)
- **[88]** Diversity-driven Data Selection for Language Model Tuning through Sparse Autoencoder (arXiv 2025)
- **[90]** EDGE: Efficient Data Selection for LLM Agents via Guideline Effectiveness (arXiv 2025)
- **[100]** Exploring the Mystery of Influential Data for Mathematical Reasoning (COLM 2024)
- **[101]** Harnessing Diversity for Important Data Selection in Pretraining Large Language Models (arXiv 2024)
- **[103]** Improving Data Efficiency via Curating LLM-Driven Rating Systems (ICLR 2025)
- **[102]** InfiAlign: A Scalable and Sample-Efficient Framework for Aligning LLMs to Enhance Reasoning Capabilities (arXiv 2025)
- **[91]** Instruction Mining: Instruction Data Selection for Tuning Large Language Models (COLM 2024)
- **[104]** IterSelectTune: An Iterative Training Framework for Efficient Instruction-Tuning Data Selection (arXiv 2024)
- **[131]** LEAD: Iterative Data Selection for Efficient LLM Instruction Tuning (arXiv 2025)
- **[105]** Learning in the Wild: Towards Leveraging Unlabeled Data for Effectively Tuning Pre-trained Code Models (ICSE 2024)
- **[29]** LESS: Selecting Influential Data for Targeted Instruction Tuning (ICML 2024)
- **[106]** MIG: Automatic Data Selection for Instruction Tuning by Maximizing Information Gain in Semantic Space (arXiv 2025)
- **[107]** MODS: Model-oriented Data Selection for Instruction Tuning (arXiv 2023)
- **[108]** Not All Documents Are What You Need for Extracting Instruction Tuning Data (arXiv 2025)
- **[109]** PASER: Post-Training Data Selection for Efficient Pruned Large Language Model Recovery (ICML 2025)
- **[110]** Pre-trained Language Model Based Active Learning for Sentence Matching (COLING 2020)
- **[111]** ProDS: Preference-oriented Data Selection for Instruction Tuning (arXiv 2025)
- **[80]** QLESS: A Quantized Approach for Data Valuation and Selection in Large Language Model Fine-Tuning (arXiv 2025)
- **[112]** RAISE: Reinforced Adaptive Instruction Selection For Large Language Models (arXiv 2025)
- **[113]** Resolving Knowledge Conflicts in Domain-specific Data Selection: A Case Study on Medical Instruction-tuning (arXiv 2025)
- **[81]** ROSE: A Reward-Oriented Data Selection Framework for LLM Task-Specific Instruction Tuning (EMNLP 2025)
- **[114]** SCAR: Data Selection via Style Consistency-Aware Response Ranking for Efficient Instruction-Tuning of Large Language Models (ACL 2025)
- **[82]** SEAL: Safety-enhanced Aligned LLM Fine-tuning via Bilevel Data Selection (ICLR 2025)
- **[115]** SELECT2REASON: Efficient Instruction-Tuning Data Selection for Long-CoT Reasoning (arXiv 2025)
- **[83]** Selective Reflection-Tuning: Student-Selected Data Recycling for LLM Instruction-Tuning (ACL 2024)
- **[116]** SHED: Shapley-Based Automated Dataset Refinement for Instruction Fine-Tuning (NeurIPS 2024)
- **[117]** SmallToLarge (S2L): Scalable Data Selection for Fine-tuning Large Language Models by Summarizing Training Trajectories of Small Models (NeurIPS 2024)
- **[118]** SPaRFT: Self-Paced Reinforcement Fine-Tuning for Large Language Models (arXiv 2025)
- **[128]** Speculative Coreset Selection for Task-Specific Fine-tuning (CoRR 2024)
- **[119]** Stratified Selective Sampling for Instruction Tuning with Dedicated Scoring Strategy (EMNLP 2025)
- **[120]** Superfiltering: Weak-to-Strong Data Filtering for Fast Instruction-Tuning (ACL 2024)
- **[121]** Synth-Empathy: Towards High-Quality Synthetic Empathy Data (arXiv 2024)
- **[122]** T-SHIRT: Token-Selective Hierarchical Data Selection for Instruction Tuning (arXiv 2025)
- **[123]** TACOS: Open Tagging and Comparative Scoring for Instruction Fine-Tuning Data Selection (arXiv 2025)
- **[124]** TAROT: Targeted Data Selection via Optimal Transport (arXiv 2024)
- **[125]** Task-Specific Data Selection for Instruction Tuning via Monosemantic Neuronal Activations (arXiv 2025)
- **[126]** The Best of Both Worlds: Bridging Quality and Diversity in Data Selection with Bipartite Graph (ICML 2025)
- **[127]** TSDS: Data Selection for Task-Specific Model Finetuning (NeurIPS 2024)
- **[94]** What Makes Good Data for Alignment? A Comprehensive Study of Automatic Data Selection in Instruction Tuning (arXiv 2023)

## Surrogate-free methods

**Definition:** Data-selection methods evaluated without a separate auxiliary scorer in the manuscript taxonomy. They may use target-model signals or model-independent criteria; this is not equivalent to “no model computations.”

### Pre-training (1 study)

- **[38]** INGENIOUS: Using Informative Data Subsets for Efficient Pre-Training of Language Models (EMNLP 2023)

### Fine-tuning (26 studies)

- **[68]** 3DS: Decomposed Difficulty Data Selection's Case Study on LLM Medical Domain Adaptation (arXiv 2024)
- **[54]** Beyond Similarity: A Gradient-based Graph Method for Instruction Tuning Data Selection (arXiv 2025)
- **[69]** Boosting LLM via Learning from Data Iteratively and Selectively (arXiv 2024)
- **[55]** CLUES: Collaborative High-Quality Data Selection for LLMs via Training Dynamics (arXiv 2025)
- **[76]** ClusterUCB: Efficient Gradient-Based Data Selection for Targeted Fine-Tuning of LLMs (arXiv 2025)
- **[64]** Data Whisperer: Efficient Data Selection for Task-Specific LLM Fine-Tuning via Few-Shot In-Context Learning (arXiv 2025)
- **[77]** DELIFT: DATA EFFICIENT LANGUAGE MODEL INSTRUCTION FINE-TUNING (ICLR 2025)
- **[56]** Efficient Data Selection at Scale via Influence Distillation (arXiv 2025)
- **[31]** Fine-Tuning Language Models via Epistemic Neural Networks (arXiv 2022)
- **[61]** From Quantity to Quality: Boosting LLM Performance with Self-Guided Data Selection for Instruction Tuning (NAACL 2024)
- **[70]** G-DIG: Towards Gradient-based Diverse and High-quality Instruction Data Selection for Machine Translation (ACL 2024)
- **[62]** Get more for less: Principled Data Selection for Warming Up Fine-Tuning in LLMs (ICLR 2024)
- **[71]** GORACS: Group-level Optimal Transport-guided Coreset Selection for LLM-based Recommender Systems (KDD 2025)
- **[57]** HyperINF: Unleashing the HyperPower of the Schulz's Method for Data Influence Estimation (CoRR 2024)
- **[53]** Improving Influence-based Instruction Tuning Data Selection for Balanced Learning of Diverse Capabilities (ICLR 2025)
- **[72]** Improving Task Diversity in Label Efficient Supervised Finetuning of LLMs (arXiv 2025)
- **[58]** Influential Language Data Selection via Gradient Trajectory Pursuit (ICLR 2025)
- **[73]** P3: A Policy-Driven, Pace-Adaptive, and Diversity-Promoted Framework for data pruning in LLM Training (arXiv 2024)
- **[67]** Pay More Attention to the Robustness of Prompt for Instruction Data Mining (arXiv 2025)
- **[74]** ResoFilter: Fine-grained Synthetic Data Filtering for Large Language Models through Data-Parameter Resonance Analysis (NAACL 2025)
- **[59]** RICO: Refined In-Context Contribution for Automatic Instruction-Tuning Data Selection (arXiv 2025)
- **[65]** STAR: Constraint LoRA with Dynamic Active Learning for Data-Efficient Fine-Tuning of Large Language Models (ACL 2024)
- **[75]** TAGCOS: Task-agnostic Gradient Clustered Coreset Selection for Instruction Tuning Data (NAACL 2025)
- **[63]** The Best Instruction-Tuning Data are Those That Fit (arXiv 2025)
- **[60]** Towards Sustainable Learning: Coresets for Data-efficient Deep Learning (ICML 2023)
- **[66]** UFO-RL: Uncertainty-Focused Optimization for Efficient Reinforcement Learning Data Selection (arXiv 2025)

## Benchmark and empirical studies

These nine entries are identified as benchmark or empirical-analysis works in Section V-B of the manuscript:

- **[132]** Active Code Learning: Benchmarking Sample-Efficient Training of Code Models (IEEE Transactions on Software Engineering 2024)
- **[133]** Active learning for reducing labeling effort in text classification tasks (BNAIC/Benelearn 2021)
- **[135]** Compute-Constrained Data Selection (ICLR 2025)
- **[134]** Large-Scale Data Selection for Instruction Tuning (CoRR 2025)
- **[140]** LIMIT: Less Is More for Instruction Tuning Across Evaluation Paradigms (arXiv 2023)
- **[137]** Rethinking Data Selection at Scale: Random Selection is Almost All You Need (ICLR 2025)
- **[138]** Rethinking Data Selection for Supervised Fine-Tuning (arXiv 2024)
- **[136]** Take the essence and discard the dross: A Rethinking on Data Selection for Fine-Tuning Large Language Models (NAACL 2025)
- **[139]** What is in Your Safe Data? Identifying Benign Data that Breaks Safety (COLM 2024)

## Catalog reconciliation notes

- **Added to the supplied README:** [31] *Fine-Tuning Language Models via Epistemic Neural Networks*; [49] *Generative Deduplication for Social Media Data Selection*; [94] *What Makes Good Data for Alignment?*
- **Corrected surrogate assignment:** [67] moved to surrogate-free; [83] moved to surrogate-based; [111] ProDS retained only as surrogate-based (it had also appeared as surrogate-free).
- **Removed duplications / out-of-corpus entries:** the second instance of [125] MONA; the extra benchmark *A Pre-trained Data Deduplication Model Based on Active Learning*; and *Token Cleaning: Fine-Grained Data Selection for LLM Supervised Fine-Tuning* (not present in the supplied manuscript bibliography).
- **Normalized metadata:** [85] *Active Learning for Effectively Fine-Tuning Transfer Learning to Downstream Task* is listed as ACM TIST (2021), consistent with the supplied manuscript, rather than Expert Systems with Applications (2025). Other venue labels largely retain the original README’s publication-version information; verify journal/conference metadata separately if citation-year alignment is required.
- **Bibliographic alias:** [92] *Neuronal Activation States as Sample Embeddings for Data Selection in Task-Specific Instruction Tuning* and [125] MONA refer to the same arXiv identifier ([2503.15573](https://arxiv.org/abs/2503.15573)) and are cataloged as one work under [125]. The manuscript currently cites [92] and [125] separately; clean this duplication in the manuscript’s bibliography and Table IV.
- **Possible classification to inspect:** [101] is listed under fine-tuning by the manuscript’s Table IV despite its pre-training-oriented title. This README preserves the manuscript’s classification pending author verification.

## Use and contributions

Use this README to locate the surveyed papers and review the documented study-identification protocol. When contributing new records or correcting bibliographic metadata, provide a source, the manuscript reference number (if applicable), the training phase, the surrogate-dependency category, and any protocol-relevant changes.

Maintained for academic research purposes.
