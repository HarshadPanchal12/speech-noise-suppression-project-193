# Speech Background Noise Suppression with Deep Learning

## MathWorks Excellence in Innovation — Project 193

**Project:** Speech Background Noise Suppression with Deep Learning  
**Project Number:** 193  
**Domain:** Deep Learning • Speech Enhancement • Audio Signal Processing  
**Platform:** MATLAB  
**Primary Toolboxes:** Audio Toolbox, Deep Learning Toolbox  
**Institution:** University of Mumbai  
**Department:** Electronics and Computer Science

---

## 1. Project Overview

This repository contains our MATLAB-based solution for **MathWorks Excellence in Innovation Project 193: Speech Background Noise Suppression with Deep Learning**.

The objective is to develop a deep-learning-based speech enhancement system that reduces background noise while preserving speech intelligibility and quality. The system follows the project requirements by combining audio signal processing with a neural network, followed by objective and subjective evaluation.

The overall processing pipeline is:

```text
Noisy Speech
     │
     ▼
Audio Pre-processing
     │
     ▼
Time-Frequency Representation (STFT)
     │
     ▼
Deep Learning Noise Suppression Model
     │
     ▼
Estimated Speech / Mask
     │
     ▼
Spectral Reconstruction
     │
     ▼
Denoised Speech
     │
     ▼
Objective + Subjective Evaluation
```

---

## 2. Project Objectives

- Suppress realistic background noise from speech recordings.
- Preserve speech intelligibility and minimize speech distortion.
- Use MATLAB Audio Toolbox and Deep Learning Toolbox for the complete workflow.
- Train and/or fine-tune a neural network using clean and noisy speech pairs.
- Evaluate performance using objective speech-enhancement metrics.
- Provide a reproducible MATLAB demo with minimal manual setup.
- Study the computational cost and inference latency of the proposed system.

---

## 3. Dataset

The project is designed around the **Microsoft Deep Noise Suppression (DNS) Challenge** dataset recommended by MathWorks.

The full DNS dataset is large and is **not included in this repository**. Instead, the repository contains scripts and/or instructions for preparing the required data and a small sample for demonstration where appropriate.

Recommended dataset source:

- Microsoft DNS Challenge: https://github.com/microsoft/DNS-Challenge

For experiments, clean speech and noise are combined at different signal-to-noise ratios (SNRs) to create noisy speech examples.

Example training conditions:

```text
Clean Speech + Noise
        │
        ├── -5 dB SNR
        ├──  0 dB SNR
        ├──  5 dB SNR
        ├── 10 dB SNR
        └── 15 dB SNR
```

---

## 4. Model and Methodology

The final submission is intended to use a **lightweight deep-learning architecture suitable for training and inference on commodity hardware** rather than depending on a high-end GPU.

The model development workflow is:

1. Read clean and noisy speech.
2. Resample audio to the configured sampling rate.
3. Divide audio into short-time frames.
4. Compute the Short-Time Fourier Transform (STFT).
5. Extract normalized time-frequency features.
6. Process the features using the selected neural network.
7. Estimate a speech/noise mask or enhanced spectral representation.
8. Reconstruct the enhanced complex spectrum.
9. Apply inverse STFT (ISTFT).
10. Evaluate the reconstructed speech.

### Planned final architecture

```text
                 Noisy Speech
                      │
                      ▼
                    STFT
                      │
                      ▼
            Time-Frequency Features
                      │
                      ▼
              CNN Feature Encoder
                      │
                      ▼
             LSTM Temporal Module
                      │
                      ▼
               Mask Estimator
                      │
                      ▼
          Enhanced Magnitude Spectrum
                      │
              + Noisy Phase
                      │
                      ▼
                    ISTFT
                      │
                      ▼
               Denoised Speech
```

> **Implementation status:** The repository currently contains earlier MATLAB denoising and FullSubNet-related code from the development/reference stage. The final submission should replace or clearly isolate incomplete experimental code and make the validated final model the default execution path.

---

## 5. Baselines and Comparison

To make the evaluation meaningful, the final project will compare the proposed model against at least one non-neural or simpler neural baseline.

Possible baselines include:

- Spectral subtraction / conventional noise suppression.
- A compact CNN denoising model.
- A MATLAB/MathWorks denoising network used strictly as a reference baseline where licensing and reproducibility permit.

The comparison will focus on both enhancement quality and computational cost.

---

## 6. Evaluation

The project will be evaluated using clean speech as the reference signal and noisy/denoised speech as the test signals.

### Objective metrics

The evaluation pipeline is intended to report:

- Input SNR
- Output SNR
- SNR improvement (ΔSNR)
- STOI — Short-Time Objective Intelligibility
- PESQ — Perceptual Evaluation of Speech Quality, where supported by the evaluation setup
- RMSE
- SI-SDR/SDR, where supported by the final implementation
- Inference time / real-time factor

Example reporting format:

| Metric | Noisy Input | Proposed Model |
|---|---:|---:|
| SNR (dB) | TBD | TBD |
| ΔSNR (dB) | — | TBD |
| STOI | TBD | TBD |
| PESQ | TBD | TBD |
| RMSE | TBD | TBD |
| Inference Time | — | TBD |

**All numerical results will be populated from reproducible experiments before final submission. No results are claimed in this README until they have been independently reproduced.**

### Subjective evaluation

Example clean, noisy and denoised audio samples will be provided for listening comparison. A small documented listening evaluation may also be used to compare perceived speech quality and residual noise.

This subjective evaluation is intended as an engineering demonstration and is not a clinical hearing assessment.

---

## 7. Real-Time / Computational Evaluation

A key engineering objective is to keep the model lightweight enough for practical inference.

For a frame stride of `T` milliseconds, the target is:

```text
Inference time < T
```

The final report will document:

- Model size
- Number of trainable parameters
- CPU/GPU used for experiments
- Average inference time
- Real-time factor (RTF)
- Memory requirements where practical

---

## 8. Repository Structure

The final repository is organized around a single reproducible entry point:

```text
.
├── run_demo.m                  # Main end-to-end demonstration
├── setup.m                     # Optional path/configuration setup
├── README.md
├── LICENSE
│
├── src/
│   ├── preprocessing/
│   ├── model/
│   ├── postprocessing/
│   └── evaluation/
│
├── training/
│   ├── train.m
│   ├── createTrainingPairs.m
│   └── configuration.m
│
├── models/
│   └── finalModel.mat
│
├── data/
│   └── sample/
│
├── examples/
│   ├── clean.wav
│   ├── noisy.wav
│   └── denoised.wav
│
├── results/
│   ├── metrics.csv
│   ├── spectrogram_noisy.png
│   └── spectrogram_denoised.png
│
├── tests/
│   └── testInference.m
│
└── docs/
    ├── architecture.png
    ├── experiments.md
    └── final_report.pdf
```

The exact folder structure may evolve during implementation, but `run_demo.m` remains the intended single entry point for evaluation.

---

## 9. Quick Start

### Requirements

- MATLAB
- Audio Toolbox
- Deep Learning Toolbox
- Signal Processing Toolbox if required by the selected preprocessing/evaluation functions
- Parallel Computing Toolbox is optional and may be used to accelerate training

### Run the demonstration

After cloning the repository and opening it in MATLAB:

```matlab
run_demo
```

The demo should:

1. Load the provided sample noisy speech.
2. Load the trained model.
3. Run the complete denoising pipeline.
4. Save the denoised output.
5. Display waveform/spectrogram comparisons.
6. Calculate available evaluation metrics.

The final repository should require no hard-coded paths to the developer's local machine.

---

## 10. Training

Training is separated from inference so that evaluators do not need to retrain the network just to verify the submission.

The training workflow is approximately:

```text
DNS Dataset
    │
    ▼
Clean/Noise Pair Preparation
    │
    ▼
Noisy Mixture Generation
    │
    ▼
STFT Feature Extraction
    │
    ▼
Network Training
    │
    ▼
Validation
    │
    ▼
Best Model Selection
    │
    ▼
finalModel.mat
```

Training configuration, random seeds where applicable, dataset preparation steps, and important hyperparameters will be documented so the experiments can be reproduced.

---

## 11. Generalization Test

The final evaluation should include noise conditions that are not identical to those used during training. This helps determine whether the model learns general speech enhancement rather than memorizing a limited set of noise recordings.

The final report will document:

- Training noise categories
- Validation noise categories
- Test noise categories
- SNR ranges
- Number of test utterances
- Per-condition results

---

## 12. Results

Final experimental results, plots, audio examples and comparisons will be added after the complete training and evaluation pipeline has been validated.

The repository will include both numerical results and qualitative visualizations such as:

```text
Noisy spectrogram  →  Model  →  Denoised spectrogram
```

Representative audio files will also be provided so that the enhancement can be directly inspected by the evaluator.

---

## 13. Reproducibility and Submission Design

This repository is designed for evaluation with minimal manual intervention.

The final submission will provide:

- A public GitHub repository.
- An open-source BSD 2-Clause or MIT license.
- A single main entry point (`run_demo.m`).
- A trained model for immediate inference.
- A small demonstration dataset/sample audio.
- Dataset download/preparation instructions for the full DNS dataset.
- Documented MATLAB/toolbox requirements.
- Reproducible evaluation scripts.
- Experimental results and methodology.
- Source code for the complete inference pipeline.

Large datasets are intentionally not committed to GitHub.

---

## 14. Project Status

**Current stage:** Development / final submission preparation.

The repository contains development/reference implementations from earlier experimentation. These are retained for development history and comparison but should not be treated as the final validated result unless explicitly identified as such.

Before final submission, the repository will be cleaned so that:

- incomplete experiments are clearly separated from the final implementation;
- the final model is the default model used by `run_demo.m`;
- all reported metrics are reproducible;
- obsolete paths and machine-specific configuration are removed;
- the README and report match the actual implementation.

---

## 15. References

1. MathWorks Excellence in Innovation — Project 193: Speech Background Noise Suppression with Deep Learning.
2. Microsoft DNS Challenge: https://github.com/microsoft/DNS-Challenge
3. H. Xiang et al., *FullSubNet: A Full-Band and Sub-Band Fusion Model for Real-Time Single-Channel Speech Enhancement*, ICASSP 2021. https://arxiv.org/abs/2010.15508
4. N. L. Westhausen and B. T. Meyer, *Dual-Signal Transformation LSTM Network for Real-Time Noise Suppression*, INTERSPEECH 2020.
5. H.-S. Choi et al., *Phase-aware Single-stage Speech Denoising and Dereverberation with U-Net*, INTERSPEECH 2020.
6. J.-M. Valin, *A Hybrid DSP/Deep Learning Approach to Real-Time Full-Band Speech Enhancement*, 2018.

Additional references used during implementation will be documented in `docs/` and in the final report.

---

## 16. Acknowledgements

We acknowledge MathWorks for providing the Excellence in Innovation Challenge Project and the Microsoft DNS Challenge project for making large-scale speech and noise data available to the research community.

---

## 17. Generative AI Disclosure

Generative AI tools may be used during development for code assistance, documentation assistance, debugging suggestions and research organization. All submitted code, experiments and results will be reviewed and validated by the project team. The team is responsible for understanding, testing and explaining the final implementation.

---

## License

This project will be released under the **MIT License** or **BSD 2-Clause License** as required by the MathWorks submission guidelines. The final repository should contain the corresponding `LICENSE` file at the repository root.
