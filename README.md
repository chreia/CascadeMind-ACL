# CascadeMind

![SemEval](https://img.shields.io/badge/SemEval-2026%20Task%204-1f6feb)
![Focus](https://img.shields.io/badge/focus-narrative%20similarity-0a7f5a)
![Approach](https://img.shields.io/badge/approach-hybrid%20neuro--symbolic-b35c00)
![Official Result](https://img.shields.io/badge/official%20Track%20A-72.75%25-2ea043)
![Python](https://img.shields.io/badge/python-3.10%2B-3776ab)

CascadeMind is the SemEval-2026 Task 4 system paper codebase for **Narrative Story Similarity**. Given an anchor story and two candidate stories, the system predicts which candidate is more narratively similar to the anchor across abstract theme, course of action, and outcome.

The public release is intentionally conservative: it exposes the cleaned Gemini runner, paper source, selected historical experiment scripts, and reproducibility tooling, while keeping raw shared-task data, generated logs, notebooks, and submission bundles out of the repository until redistribution rights are confirmed.

## Paper

- Canonical source: [`paper/latest-paperfeb12026/semeval2026_final.tex`](paper/latest-paperfeb12026/semeval2026_final.tex)
- Build notes: [`paper/README.md`](paper/README.md)
- Public code URL for the paper: `https://github.com/chreia/CascadeMind-ACL`

The camera-ready paper should treat the official shared-task submission as the main result:

| Result type | Split | Accuracy | Notes |
| --- | --- | ---: | --- |
| Official submission | Track A test | 72.75% | Listed as rank 10 in the task overview table |
| Post-hoc diagnostic run | Released Track A test labels | 73.0% | Diagnostic only, not a change to the official standing |
| Paper-era cascade diagnostics | Development subset | 81.0% | Useful for routing analysis; denominator differs from full dev baselines |

## What To Run

Use a fresh, rotated Gemini API key. Do not reuse any key that has appeared in chat, logs, git history, or screenshots.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

cp .env.example .env
# Fill in only the keys you need, then load them into your shell.
set -a
source .env
set +a
```

Primary entrypoints:

```bash
python best.py
python baseline.py
python train_ensemble.py
```

Camera-ready experiment runner:

```bash
python scripts/run_camera_ready_experiments.py \
  --data data/dev_track_a.jsonl \
  --suite balanced \
  --max-examples 5
```

The scripts read credentials from environment variables directly; they do not auto-load `.env`.

## Data Policy

The SemEval task data and archived submissions are not tracked in this public-release branch. See [`data/README.md`](data/README.md) for expected filenames, row counts, and checksums from the previous LFS pointers.

Local generated outputs should live under ignored `artifacts/runs/` directories. If you run experiments for the camera-ready paper, keep the generated manifest with the model IDs, parameters, token/call counts, row counts, and git commit.

## Repo Map

| Path | Purpose |
| --- | --- |
| [`best.py`](best.py) | Cleaned Gemini bidirectional evaluator |
| [`baseline.py`](baseline.py) | Minimal Gemini structured-output baseline |
| [`train_ensemble.py`](train_ensemble.py) | Multi-signal symbolic ensemble trainer |
| [`experiments/`](experiments) | Selected Gemini historical variants and ablations; not all are public-release entrypoints |
| [`scripts/run_camera_ready_experiments.py`](scripts/run_camera_ready_experiments.py) | Manifested Gemini rerun harness |
| [`scripts/check_release.py`](scripts/check_release.py) | Public-release and camera-ready sanity checks |
| [`paper/`](paper) | System paper source and style files |

## Security

Do not commit API credentials, local `.env` files, raw task data, generated logs, or submission bundles. Any credential that has appeared in chat, logs, screenshots, or old private history should be considered compromised and rotated.

Before publishing, run:

```bash
python scripts/check_release.py
```

The public branch is published as a clean one-commit history so old private development artifacts are not reachable from the public remote.

## Citation

```bibtex
@inproceedings{kawada-holyoak-2026-cascademind,
  title     = {CascadeMind at SemEval-2026 Task 4: A Hybrid Neuro-Symbolic Cascade for Narrative Similarity},
  author    = {Kawada, Sebastien and Holyoak, Dylan},
  booktitle = {Proceedings of the 20th International Workshop on Semantic Evaluation (SemEval-2026)},
  year      = {2026},
  address   = {San Diego, CA, USA}
}
```
