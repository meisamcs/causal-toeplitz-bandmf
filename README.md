# Causal Toeplitz Noise Shaping for Amplified Banded DP-SGD

This repository hosts the frozen reproducibility artifact accompanying the ICASSP paper **Causal Toeplitz Noise Shaping for Amplified Banded DP-SGD**.

The primary mechanism uses a public causal, lower-triangular, `K`-tap Toeplitz strategy. Its streaming implementation filters Gaussian innovations through the causal inverse and adds the resulting correlated noise to the current clipped batch gradient; it does not retain a history of private gradients or deterministically smooth the gradient signal.

## Reproducibility package

Download the complete 113-file artifact from the [v1.0.0 release](https://github.com/meisamcs/causal-toeplitz-bandmf/releases/tag/v1.0.0).

SHA-256:

`6e83fbfbd7cbe16e5bcd4c70b3b438aeedf9508c45bebeb67b3bc486d9166441`

The archive contains:

- frozen training and strategy-design code;
- public Toeplitz designs for `epsilon in {0.5, 0.75, 1.0, 1.5, 2.0}`;
- pre-run manifests, code hashes, and the public MNIST split-index hash;
- per-seed DP-SGD, moving-average, cyclic-identity, and BandMF results;
- paper figures, aggregated tables, and pinned environments.

## Headline five-seed confirmation

At `epsilon=1.5`, `B=300`, `C=0.5`, and `K=7`:

| Method | Overall accuracy | Class-8 accuracy |
|---|---:|---:|
| Standard DP-SGD | 90.31% | 55.79% |
| Cyclic identity (`G=I`) | 73.24% | 4.35% |
| Causal Toeplitz BandMF | **92.53%** | **63.39%** |

The paired BandMF-minus-DP-SGD overall gain is +2.22 percentage points, with a two-sided 95% Student-t confidence interval of [0.12, 4.32].

## Quick verification

After downloading and extracting the release:

```bash
cd causal-toeplitz-bandmf
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
PYTHONPATH=code python -m unittest discover -s code -p 'test_*.py'
```

All 16 included unit and integration tests pass. The packaged README gives the complete MNIST preparation, privacy-budget sweep, five-seed confirmation, MA(10) control, and public-strategy rebuild commands.

## Privacy scope

The privacy statement assumes public and fixed `G`, `K`, `C`, `T`, and cyclic grouping rules; record-separable clipping; zero-out adjacency; and hidden, independent sampling and Gaussian randomness. Privacy calibration uses a PLD accountant. The release contains no private random-number-generator states or model checkpoints.
