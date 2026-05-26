# Policy Ambiguity Is Nocuous

Code and data for "Policy Ambiguity Is Nocuous: Clause-Level Causal Effects on Customer-Service Agent Safety" (EMNLP 2026 submission).

## Abstract

LLM-based agents increasingly operate under natural-language policy specifications that inevitably contain ambiguity, yet no prior work causally links ambiguity to safety violations. Using a matched-pair design on τ²-bench with 300 clause pairs spanning a six-type taxonomy evaluated across five frontier models, we show that ambiguous clauses increase violations by +34.0 percentage points, an effect that generalizes directionally across benchmarks, domains, and model families. Ambiguity *type* predicts *how* agents fail—each type maps to a dominant failure mechanism—but not *whether* they fail, orienting the taxonomy as a diagnostic instrument rather than a risk classifier. Error analysis reveals that agents almost never recognize ambiguity and converge on identical permissive violations across architecturally diverse models, a phenomenon we term *convergent nocuity*: agreement-based safety monitoring is blind to these systematic errors. These findings motivate clause-level policy auditing over type-level risk screening.

## Setup

```bash
pip install -r artifacts/requirements.txt
```

Set environment variables:
```bash
export OPENROUTER_API_KEY=your_key_here
# Or use OpenAI-compatible endpoint
export OPENAI_API_KEY=your_key_here
```

## Repository Structure

- `artifacts/` — Main experiment code and data
  - `harness.py` — Core evaluation harness (tau-bench integration)
  - `judge.py` — Cross-judge violation assessment pipeline
  - `injection_pipeline.py` — Clause pair injection into policy documents
  - `analyze.py` — Statistical analysis module
  - `run_full_study.py` — Main study runner (5 models x 300 clause pairs)
  - `full_study/analysis/` — Statistical analysis scripts (variance decomposition, TOST, nocuity)
  - `sopbench/` — SOPBench cross-benchmark replication
  - `human_annotation/` — Human annotation tools, codebook, and annotator data
  - `reannotation/` — Re-annotation study materials
  - `figures/paper/` — Figure generation scripts
  - `prevalence_audit/` — Real-world policy ambiguity prevalence study
  - `convergence_analysis/` — Cross-model convergence and attractor analysis
  - `cross_judge_validation/` — Multi-judge validation pipeline
  - `disambiguation_intervention/` — Disambiguation intervention experiment
  - `controlled_effect_size/` — Controlled effect size computation
  - `residual_decomposition/` — Variance residual decomposition
  - `clause_templates*.json` — Generated clause pair data (ambiguous/unambiguous)

## Reproducing Results

### Main Study (5 models x 300 clause pairs)
```bash
python artifacts/run_full_study.py
```

### Statistical Analysis
```bash
# Variance decomposition (clause vs. model vs. interaction)
python artifacts/full_study/analysis/compute_clause_variance_decomposition.py

# Systematic nocuity across 5 models
python artifacts/full_study/analysis/compute_systematic_nocuity_5model.py

# TOST equivalence testing
python artifacts/full_study/analysis/compute_tost_5model.py

# Cross-type hierarchy analysis
python artifacts/full_study/analysis/compute_five_type_hierarchy.py
```

### Human Agreement Analysis
```bash
# Inter-annotator agreement (collapsed kappa)
python artifacts/compute_collapsed_kappa_v3.py

# Cramer's V and NMI
python artifacts/compute_cramer_nmi.py
```

### SOPBench Cross-Benchmark Replication
```bash
cd artifacts/sopbench
pip install -r requirements.txt
python run_full_experiment.py
```

### Prevalence Audit
```bash
python artifacts/prevalence_audit/extract_clauses.py
python artifacts/prevalence_audit/compute_stats.py
```

### Figures
```bash
python artifacts/figures/paper/gen_fig1_overview.py
python artifacts/figures/paper/gen_fig3_heatmap_5model.py
# gen_fig2_forest_plot.py retained for completeness but fig2 was removed from the final paper
```

## Data

- `artifacts/clause_templates_full.json` — Full set of 300 clause pairs (6 ambiguity types)
- `artifacts/clauses_*.json` — Clause subsets by type and experiment phase
- `artifacts/sopbench/data/` — SOPBench task data (7 domains)
- `artifacts/human_annotation/` — Annotation sheets, codebook, and annotator responses
- `artifacts/prevalence_audit/raw_docs/` — Real-world policy documents (airlines, retail)
- `artifacts/analysis_*.json` — Pre-computed analysis results

## Requirements

- Python >= 3.10
- OpenAI-compatible API access (for model evaluation)
- See `artifacts/requirements.txt` for Python dependencies
