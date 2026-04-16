# cursor-rules-ml

A curated set of **Cursor IDE rules** and best practices for senior-level ML engineering and data science work in Python.

## 📋 Overview

This repository contains comprehensive `.cursorrules` files that define coding standards, architectural patterns, and production-grade practices for machine learning and data science projects. These rules enforce reproducibility, clarity, and best practices across your entire codebase.

Three rule sets are provided:

- **`.cursor-ml-rules.md`** — Comprehensive ML/Senior Data Science guidelines
- **`.cursor-DS-rules.md`** — Alternative DS-focused configuration
- **`.cursor-ai-engg-rule.md`** — Senior AI Engineer rules emphasizing production agents and systems

## 🎯 Purpose

These rules establish:
- **Code Quality**: Linting, formatting, type checking, and testing standards
- **Project Structure**: Organized layouts for data, models, configs, and tests
- **Data Practices**: Schema validation, exploratory analysis, feature engineering
- **ML Workflows**: Experiment tracking, model evaluation, hyperparameter tuning
- **Reproducibility**: Seed management, dependency pinning, artifact versioning
- **Production Safety**: Anti-patterns prevention, security, data governance

## 🚀 Quick Start

1. **Copy the rules file to your project:**
   ```bash
   cp .cursor-ml-rules.md /path/to/your/ml-project/.cursorrules
   ```

2. **Or use in Cursor IDE settings:**
   - In Cursor, go to **Settings** → **Rules**
   - Paste the content from `.cursor-ml-rules.md`
   - Save and reload the editor

3. **Start coding** — Cursor will apply these rules to all conversations and suggestions

## 📚 Key Sections

### 1. **Identity & Mindset**
Senior ML Engineer perspective: production-grade, reproducible, well-reasoned Python with clarity over cleverness.

### 2. **Python Style** (Python 3.10+)
- Type annotations on all function signatures
- f-strings, union types (`X | None`)
- Max line length: 100 chars
- `pathlib.Path` for file operations
- Dataclasses and Pydantic for structured data

### 3. **Project Structure**
```
project/
├── data/
│   ├── raw/        # Immutable original data
│   ├── interim/    # Cleaned, intermediate
│   └── processed/  # Final features for modelling
├── notebooks/      # Exploration only
├── src/
│   ├── data/       # Ingestion, cleaning, pipelines
│   ├── models/     # Model definitions, trainers
│   └── utils/      # Shared helpers
├── configs/        # YAML/Hydra configs
├── tests/          # Mirrors src/ structure
└── scripts/        # Entry-point CLI scripts
```

### 4. **Data & Pandas**
- Vectorized operations (no Python-level loops)
- Schema validation (pandera, Great Expectations)
- Inspect dtypes and nulls: `df.info()`, `df.isnull().sum()`
- For large data (>1M rows): chunked reads, Polars, DuckDB

### 5. **Feature Engineering**
- sklearn-compatible transforms (fit/transform API)
- Mandatory leakage prevention
- Document each feature: origin, formula, caveats
- Separate raw and engineered features

### 6. **Modelling**
- Wrap experiments in functions/classes
- sklearn Pipelines (preprocessing + model)
- Set and log all random seeds
- Multiple metrics; always include a baseline
- Hyperparameter search: Optuna or `RandomizedSearchCV`

### 7. **Experiment Tracking**
- Log everything: params, metrics, artifacts (MLflow/W&B/Comet)
- Descriptive run names with dataset version & git hash
- Version all model artifacts (never overwrite)
- Save fitted pipeline + repro script

### 8. **Evaluation & Validation**
- Stratified k-fold for imbalanced classification
- Confidence intervals (bootstrap, cross-val std)
- Multiple visualizations: confusion matrix, ROC/PR, calibration
- Check for data drift before trusting metrics
- Respect temporal ordering in time-series

### 9. **Reproducibility**
- Pin all dependencies (exact versions)
- Environments via `uv` or conda
- Seed everything at top of each script/cell
- Version-controlled YAML/TOML configs (Hydra)
- DVC for large data/model artifacts

### 10. **Code Quality**
- **Linting**: `ruff`
- **Formatting**: `black` (line-length 100)
- **Type checking**: `mypy --strict` on `src/`
- **Pre-commit hooks**: ruff, black, mypy, pytest
- Google-style docstrings for all non-trivial functions

### 11. **Testing**
- Mirror `src/` layout under `tests/`
- Unit-test pure functions with small fixtures
- Integration-test full pipelines end-to-end
- Mark slow tests: `@pytest.mark.slow`
- Target: ≥80% coverage on `src/`, 100% on critical transforms

### 12. **Logging & Observability**
- Use `logging` (stdlib) or `loguru`, never `print()`
- Log dataset shapes, nulls, class distributions at ingestion
- Log metrics at each epoch/CV fold
- Raise `ValueError`/`RuntimeError` with actionable messages

### 13. **Performance**
- Profile before optimizing (`line_profiler`, `py-spy`)
- Use `joblib.Parallel` for CPU-bound loops
- NumPy operations over Python loops
- Minimize host↔device transfers for GPU code
- Cache expensive intermediates (`.parquet`, `.feather`)

### 14. **Security & Data Governance**
- Never commit data files, credentials, API keys
- Use `.env` + `python-dotenv`
- Anonymize PII before logging/storage
- Document in DATA_CARD.md (provenance, license)
- Model cards: intended use, limitations, biases

### 15. **Anti-Patterns to Avoid**
- Fitting scalers/encoders on full dataset before train/test split
- Using accuracy as sole metric for imbalanced data
- Magic numbers (use named constants)
- Silently dropping nulls without logging
- `import *` anywhere
- Global model state
- Hardcoded file paths
- Training code running at module import time

### 16. **Comments & Documentation**
- Explain *why*, not *what*
- Google-style docstrings (Args, Returns, Raises)
- One markdown cell per section in notebooks
- Keep CHANGELOG.md with semantic versioning

### 17. **Git Workflow**
- Conventional commits (`feat:`, `fix:`, `exp:`, `data:`)
- Feature branches (never direct to `main`)
- Squash experiment commits before merging
- Tag releases: `v1.2.0-model`

## 💡 Usage Tips

### For New Projects
1. Create your project structure following the template above
2. Copy `.cursor-ml-rules.md` as `.cursorrules` in your project root
3. Configure your IDE to use the rules file
4. Initialize git, add `.gitignore`, and start coding

### For Existing Projects
1. Review the rules against your current codebase
2. Plan refactoring in phases (prioritize data pipelines, tests)
3. Gradually adopt the structure and practices
4. Use pre-commit hooks to enforce style automatically

### Cursor Workflow
- Use Cursor's chat to ask questions about the rules
- Cursor will apply these guidelines to all code suggestions
- Reference specific sections in prompts for focused guidance
- Example prompt: *"Implement a feature engineering pipeline following the sklearn-compatible transforms approach from section ── FEATURE ENGINEERING"*

## 🔒 Key Principles

1. **Production-Grade Code**: No shortcuts, always production-ready
2. **Reproducibility First**: Seeds, versions, configs, artifacts tracked
3. **Data Quality**: Validate early, inspect often, prevent leakage
4. **Experimentation Structure**: Functions/classes, never loose training code
5. **Testing & Monitoring**: ≥80% coverage, log everything, track experiments
6. **Security by Default**: PII anonymized, credentials isolated, data governance
7. **Performance & Scale**: Profile before optimizing, use appropriate tools for data size

## 📂 Files in This Repository

| File | Description |
|------|-------------|
| `.cursor-ml-rules.md` | Main comprehensive ML/DS rules file |
| `.cursor-DS-rules.md` | Alternative DS-focused rules file |
| `.cursor-ai-engg-rule.md` | Senior AI Engineer rules focusing on production agents and ML systems |
| `README.md` | This file |
| `skills/` | Future: custom Cursor skills and extensions |

## 🎓 Who This Is For

- **Senior ML Engineers** building production models
- **Data Scientists** wanting reproducible, scalable workflows
- **Teams** needing consistent standards across projects
- **Anyone** adopting Cursor IDE for Python/ML work

## 📖 Learning Resources

The rules reference several tools and frameworks:
- **Testing**: pytest, hypothesis
- **Linting**: ruff
- **Formatting**: black
- **Type Checking**: mypy
- **Experiment Tracking**: MLflow, Weights & Biases, Comet
- **ML Libraries**: scikit-learn, pandas, NumPy, PyTorch, TensorFlow
- **Data Validation**: pandera, Great Expectations
- **Configuration**: Hydra, YAML/TOML
- **Versioning**: DVC, git

## ⚙️ Configuration & Customization

These rules are intentionally opinionated but can be customized:

1. **Edit variables**: Change line-length (100), coverage targets (80%), Python version (3.10+)
2. **Adjust tools**: Swap `black` for `autopep8`, `mypy` for `pyright`
3. **Add sections**: Insert company-specific standards or domain rules
4. **Remove sections**: If a practice doesn't fit your workflow

## 🤝 Contributing

To improve these rules:
1. Test changes in a real project
2. Document rationale for new guidelines
3. Keep rules concise and actionable
4. Maintain the opinionated, production-first mindset

## 📝 License

These rules are open for use and modification. Consider licensing your project separately.

---

**Last Updated**: April 2026  
**Python Version**: 3.10+  
**Cursor IDE Compatibility**: Latest versions