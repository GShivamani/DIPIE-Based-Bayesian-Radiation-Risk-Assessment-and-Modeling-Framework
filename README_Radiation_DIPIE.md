# ☢️ Radiation Risk Assessment via DIPIE Model

> A physics-informed probabilistic framework for assessing radiation exposure risk in humans using the **Dual Intervened Poisson-Intervened Exponential (DIPIE)** model, powered by Bayesian Neural Networks and MCMC sampling.

**Applicative Project — SEM 6 | Group 98**  
**Mentor:** Dr. Uday Chandra

**Team Members:**
| Name | ID |
|---|---|
| Mithil Bhargav | 23WU0101043 |
| Naina Vismi | 23WU0102137 |
| Abhiram Rushi | 23WU0102136 |
| Aditya Kumar | 23WU0101118 |
| Shivmani | 23WU0101045 |

---

## 📌 Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [What is the DIPIE Model?](#what-is-the-dipie-model)
- [Methodology](#methodology)
- [Input Parameters](#input-parameters)
- [Algorithmic Flow](#algorithmic-flow)
- [Novelty](#novelty)
- [Literature References](#literature-references)
- [Implementation](#implementation)

---

## Overview

This project develops a **mathematical and probabilistic model** to assess radiation risk for individuals exposed to radioactive elements. Unlike traditional deterministic models, the DIPIE framework accounts for **biological interventions**, **latent activities**, and **chelation effects**, combining them into a unified Bayesian posterior for robust, real-world risk estimation.

The core innovation is the fusion of two statistical distributions:
- **Intervened Poisson** — for discrete damage events (biomarker readings, organ damage counts)
- **Intervened Exponential** — for time-to-event clinical outcomes (symptoms, organ injuries)

---

## Objectives

1. **Design a mathematical model** for a scenario where a person is exposed to radioactive elements
2. **Consider all possible parameters** for the most accurate outcome
3. **Design the algorithmic flow** to train the model using synthetic datasets, embedded on a **Physics-Informed Bayesian Neural Network (PIBNN)**
4. **Implement, evaluate, and optimize** the trained model on real-world-like datasets
5. **Compare** the nature of the DIPIE model against pre-existing radiation risk models

---

## What is the DIPIE Model?

**DIPIE** stands for **Dual Intervened Poisson-Intervened Exponential**.

It is a novel probabilistic model designed to capture the **uncertainty of radioactive decay** under biological interventions inside the human body.

| Component | Models | Example Observations |
|---|---|---|
| **Intervened Poisson** | Discrete biological damage events | Biomarker readings, organ damage counts, DNA strand breaks |
| **Intervened Exponential** | Time-to-adverse clinical event | Symptom onset, organ injury progression |
| **ODE Backbone** | Radioactive decay + biological clearance | Activity in body over time |
| **MCMC Sampling** | Posterior inference | Marginal risk estimates with uncertainty |

---

## Methodology

The model is built in a step-by-step mathematical pipeline:

### Step 1 — Physics Decay ODE
Form a differential equation for radioactive atoms present inside the human body using the **physics decay law**:

```
dN/dt = -λ·N
```

### Step 2 — Biological Intervention
Extend the base ODE by adding **biological clearance** (metabolic elimination) and **chelation** (medically-induced removal) terms to model realistic in-body decay rates.

### Step 3 — Intervened Poisson Component
Model **discrete damage counts** (biomarker readings, cellular damage) driven by the decay rate λ_eff derived from Step 2:

```
Damage Counts ~ Poisson(λ_eff · t)
```

### Step 4 — Intervened Exponential Component
Model **adverse clinical events** (symptoms, organ injuries) as an exponential survival process intervened by the dose-rate:

```
Time-to-Event ~ Exponential(μ · dose_rate)
```

### Step 5 — Observation Device Distributions
Design likelihood distributions and **physics-informed priors** for each measurement device:
- Dosimeter readings
- Whole Body Counter (WBC)
- Organ scanner outputs

### Step 6 — Joint Posterior via MCMC
Combine all component distributions into a **joint posterior distribution** and extract marginal posterior samples via **Markov Chain Monte Carlo (MCMC)** simulation.

### Step 7 — Risk Assessment
Evaluate the posterior samples against pre-defined **clinical thresholds** to produce a final risk classification (low / moderate / high / critical).

---

## Input Parameters

The model accepts 11 categories of input parameters for maximum accuracy:

| # | Parameter | Description |
|---|---|---|
| 1 | **Dosimeter Readings** | External radiation dose measurements |
| 2 | **Biomarkers** | Blood/urine markers indicating internal exposure |
| 3 | **Internal Contamination** | Radionuclide intake levels |
| 4 | **Age & Health Status** | Physiological baseline of the individual |
| 5 | **Exposure Duration & Type** | Acute vs. chronic, alpha/beta/gamma source |
| 6 | **Protective Equipment Usage** | Shielding factors applied |
| 7 | **Environmental Monitoring** | Ambient radiation field measurements |
| 8 | **Genetic Susceptibility** | Individual radiosensitivity factors |
| 9 | **Medical History** | Pre-existing conditions affecting dose response |
| 10 | **Device Calibration** | Instrument uncertainty and correction factors |
| 11 | **Physics Priors** | Known decay constants, half-lives, S-factors |

---

## Algorithmic Flow

The complete algorithmic implementation is available on Google Colab:

🔗 **[Open in Google Colab](https://colab.research.google.com/drive/10T04eCWKjCDu3Dc0_39yAc7zSm1opstC#scrollTo=ST1ZdoFZ2FDU)**

**High-level flow:**

```
Input Parameters
      │
      ▼
Physics ODE (Decay + Biological Clearance)
      │
      ▼
Synthetic Dataset Generation
      │
      ▼
Physics-Informed Bayesian Neural Network (PIBNN) Training
      │
      ▼
DIPIE Joint Posterior Construction
      │     ├── Intervened Poisson (damage counts)
      │     └── Intervened Exponential (clinical events)
      │
      ▼
MCMC Sampling (Marginal Posteriors)
      │
      ▼
Threshold Evaluation → Risk Classification
      │
      ▼
Model Evaluation & Comparison with Existing Models
```

---

## Novelty

What makes DIPIE different from existing models:

| Feature | Existing Models | DIPIE Model |
|---|---|---|
| **Decay modeling** | Deterministic | Probabilistic (ODE + uncertainty) |
| **Biological factors** | Limited or absent | Full chelation + biological clearance |
| **Distribution type** | Single (Poisson OR Exponential) | Dual (Poisson AND Exponential combined) |
| **Latent activities** | Not modeled | Explicitly included |
| **Output** | Point estimate | Full posterior with uncertainty bounds |
| **Computational cost** | High (full simulation) | Low (MCMC sampling) |
| **Flexibility** | Fixed parameters | Adaptable to individual patient profiles |

> *"Due to the consideration of multiple factors and detailed mapping of input parameters, the DIPIE model is robust and flexible, aiming to provide accurate and reliable risk output at lower computational cost."*

---

## Literature References

### Dosimetry & Single-Compartment ODE Models
- **ICRP Publication 119** — Dose coefficient tables and single-compartment modeling background (2012)
- **MIRD Pamphlets No.1, No.11, No.21, No.28** — MIRD schema, S-value definitions, cumulated activity to organ dose
- **Knoll, G.F.** — *Radiation Detection and Measurement*, 4th ed. (2010)
- **IAEA / NRC Guidance** — Practical monitoring and dose assessment procedures (IAEA Safety Reports, NRC NUREG/CR)

### Epidemiology & Dose-Response Modeling
- **BEIR VII (NRC, 2006)** — *Health Risks from Exposure to Low Levels of Ionizing Radiation* — ERR concepts, cohort analyses
- **Preston, D.L. et al.** — *Solid Cancer Incidence in Atomic Bomb Survivors: 1958–1998*, Radiation Research (2007)
- **UNSCEAR Reports** (2008, 2020) — Global assessments of exposure and effects
- **Cox, D.R.** — Proportional hazards theory and partial likelihood for hazard models

### Biokinetic & Compartmental Models
- **Leggett, R.W.** — Biokinetic models for radiological protection; evolution of ICRP biokinetic models
- **ICRP Biokinetic Model Series** — Systemic models and parameters for dose computation
- **MIRDcalc / MIRDOSE / DCAL** — Software implementations of PBPK and dose integration

---

## Implementation

**Language / Framework:** Python (Google Colab)

**Key libraries used:**
- `NumPy` / `SciPy` — ODE solving and numerical computation
- `PyMC` or `Stan` — MCMC sampling for posterior inference
- `TensorFlow` / `PyTorch` — Physics-Informed Bayesian Neural Network
- `Matplotlib` / `Seaborn` — Visualization of posterior distributions and risk curves

**Dataset:** Synthetic datasets generated from the physics ODE under varied parameter conditions

---

## License

This project was developed for academic purposes as part of Semester 6 Applied Project work.

---

*Developed by Group 98 under the guidance of Dr. Uday Chandra.*
