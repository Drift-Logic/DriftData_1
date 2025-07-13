# Persuasive Essay Argument-Mining Sample Dataset

This is a sample dataset that includes 150 annotated essays, 75 drawn from Set 1 and 75 from Set 2.  It  provides a preview of a curated corpus of **synthetically‑generated, argument‑annotated persuasive essays**. Each entry contains the full essay text plus fine‑grained annotations of *major claims, supporting/attacking claims, premises,* and the **directed relations** that connect them. The files are stored one‑per‑essay as UTF‑8 JSON.

---

## 1. What this dataset is

| Aspect                | Details                                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Domain**            | General‑interest persuasive writing (current events, ethics, policy, technology, etc.)                                 |
| **Language**          | English                                                                                                                |
| **Size (v1)**         |  = **150** essays                                                                             |
| **Annotation scheme** | BIO span labels (B‑MAJOR, I‑MAJOR, B‑CLAIM, I‑CLAIM, B‑PREM, I‑PREM, O) + `supports` / `attacks` / `no‑relation` edges |
| **File structure**    | `origin_text`, `subtopic`, `argument_units_raw`, `argument_units_clean`, `relations`, `meta`                           |

### Minimal JSON example

```jsonc
{
  "origin_text": "Lobbying plays a pivotal role…",    // full essay
  "subtopic": "The Effect of Lobbying on Policy Making",
  "argument_units_clean": [
    {"role":"major_claim","text":"Lobbying plays a pivotal role…","offsets":[0,126]},
    {"role":"claim","text":"Proponents argue that lobbying…","offsets":[146,276]},
    …
  ],
  "relations": {
    "root": [
      {"source":"Lobbying plays a pivotal role…",
       "target":"Proponents argue that lobbying…",
       "relation":"supports"},
      …
    ]
  },
  "meta": {"ok_ratio":1.0,"timestamp":"2025‑06‑18T11:40:32Z","uuid":"…"}
}
```

---

## 2. How the dataset was generated

The corpus is produced by an **AI Agent Network** that combines large‑language models with deterministic post‑processing. A condensed pipeline:

1. **Topic & Sub‑topic Seeding**\
   • A scheduler samples diverse macro‑topics (politics, science, lifestyle…) and sub‑topics via keyword expansion to maximise topical variety.
2. **Essay Generation**\
   • An LLM author agent (e.g. GPT‑4o) generates a persuasive essay between *200–600 words*, conditioned on target stance and length bucket.
3. **Argument Unit Extraction**\
   • A second agent segments the essay into *major claim, claims,* and *premises* using a zero‑shot prompting template.
4. **Relation Mining**\
   • A relation agent predicts pairwise `supports` / `attacks` edges, building a directed graph rooted at the major claim.
5. **Parsing & Cleaning**\
   • Offsets are matched back to the original text; duplicate or overlapping spans are merged; tokens are converted to BIO labels for ML consumption.
6. **Independent QA & De‑Hallucination**\
   • A separate verifier model cross‑checks:
   - every span text matches its offset;
   - relation endpoints exist;
   - no contradictory edges;
   - Failures trigger automatic deletion.

7. **Metadata Enrichment**\
   • Each JSON gains provenance (`uuid`, `timestamp`, agent versions, ok\_ratio) to support auditability.

> **Why synthetic?** Public argumentative corpora are scarce and often limited. This pipeline allows us to dial complexity, stance distribution, and topic coverage.

---

## 3. Intended use‑cases

- **Fine‑tuning sequence‑labelling models** for argument unit detection (BIO tagging) or joint token + relation heads.
- **Benchmarking argument extractors** on unseen topics, including graph‑level relation F1.
- **Few‑shot / in‑context prompting** where one or two JSON examples supply structured demos.
- **Curriculum learning**—start with essays of shorter length or simpler graphs, progressively add harder samples.
- **Data augmentation** when combined with real‑world corpora to improve domain robustness.

---


## 4. Licence & citation

This dataset is released under the **CC BY-NC Licence + Attribution**. If you use it in research, please cite:

```text
@misc{driftlogic2025persuasive,
  title  = {Synthetic Persuasive Essay Argument‑Mining Dataset},
  author = {Driftlogic AI},
  year   = {2025},
  note   = {https://driftlogic.ai}
}
```

Commercial Use?
If you’d like to use this dataset for commercial training, products, or tools, purchase the full Pro Bundle at [driftlogic.ai].

---

## 5. Contact / contributions

Questions, issues email [**founder@driftlogic.ai**](mailto\:founder@driftlogic.ai).

*Happy training!*

