Here is a **clean, professional, and accurate README.md** that fully explains your repo, the scripts, the architecture, how they interrelate, and the purpose of each file.

You can copy/paste this directly into your GitHub repo.

---

# 📈 **srchulo Trading Indicators Repository**

This repository contains the full suite of custom Pine Script indicators powering the **A+ Flush System**, **Momentum Engine**, and **Srchulo Futures Framework**.
All scripts are written in **TradingView Pine Script v5** and designed to work together as a cohesive trading ecosystem.

---

## 🚀 **Overview**

This repo houses the indicators used for:

* **A+ Flush continuation trades**
* **Momentum state detection (FlushMomentum engine)**
* **Futures scalping models (Strict & Relaxed)**
* **Trend orchestration combining multiple signals**
* **Adaptive settings such as fmTrendStrength, fmLookback, EOD cutoff, etc.**

The core logic is centralized around the **Flush Momentum state engine**, which is kept in sync across the main flush indicator.

---

## 📁 **Repository Structure**

### **`flush-indicator.pine`**

> **Primary A+ Flush System**
> This is the main indicator used in live trading.

It:

* Generates **Flush Long / Flush Short** entries
* Uses **FlushMomentum** for trend state + early-in-trend filters
* Integrates ATR, volume thrust, and multi-timeframe slope logic
* Includes all your refined rules:

  * fmTrendStrength
  * fmLookback
  * ATR-location filters
  * Disqualifying conditions
  * Time-of-day gating
  * Moxie-style slope and color alignment
* Updated recently to set **fmTrendStrength = 0.45**

This is the script you trade every day.

---

### **`flush-momentum.pine`**

> **The core momentum engine for Flush trading**

This is a standalone visualization of the recurrent FlushMomentum model:

* EMA model: 40 / 105 / 170
* Recurrent update: `osc_t = base_step + k * osc_{t-1}`
* CSV-tuned coefficients (`a0, a1, a2, a3, k`)
* Defines:

  * Strong trend zones
  * Neutral zones
  * Momentum slope
  * TrendUp / TrendDown over N bars
* Used in all Flush logic as the state engine.

⚠️ **You must keep the momentum settings identical between this file and `flush-indicator.pine`.**
(This repo already includes comments warning about this.)

---

### **`srchulo-futures-strict.pine`**

> **Strict rule-based futures system**

Built for MNQ/MES precision.

Includes:

* Strict A+, Flush, and momentum criteria
* Hard time-of-day cutoffs
* Tight ATR extensions
* Zero-cross alignment
* Higher standard required for entry
* EOD cutoff now adjustable (recent update)

This is the “highest accuracy” version.

---

### **`srchulo-futures-relaxed.pine`**

> **Relaxed version of the futures system**

More flexible than the Strict version:

* Allows more borderline A+ patterns
* Smoother Moxie-style slope gating
* Wider ATR tolerance
* Adjustable end-of-day cutoff
* More forgiving continuation logic

Used for discovery, research, and catching more setups.

---

### **`srchulo-momentum`**

> **Experimental / supplemental momentum script**

This is an earlier or alternative momentum engine used during development.
Contains tests for:

* Alternative slope gating
* Different EMA stack normalization
* Variants of FlushMomentum
* Visual debugging tools

Useful for experimentation, but **primary logic now lives in `flush-momentum.pine`.**

---

### **`srchulo-orchestrator.pine`**

> **High-level orchestration of multiple signals**

Combines multiple components:

* FlushMomentum
* A+ logic
* Moxie-style slopes
* Trend alignment
* ATR + volume + ribbon context
* Optional gating layers

The orchestrator is designed to unify signals from multiple indicators into one combined model or dashboard.

---

### **`README.md`**

This file (you’re reading the updated version).

---

## 🔗 **How the Scripts Work Together**

### **1. FlushMomentum (state engine)**

* Defines trend strength
* Used by **flush-indicator**
* Determines early vs late entries
* Provides fmTrendStrength, fmLookback logic

### **2. flush-indicator (signal generator)**

* Reads FlushMomentum
* Computes everything else (ATR, Moxie slopes, volume thrust)
* Outputs **A+ Flush L/S** signals
* Main live-trading tool

### **3. Futures Strict & Relaxed**

* Apply the same logic but specialized for MNQ/MES
* Strict = high accuracy
* Relaxed = more setups, more experimentation

### **4. Orchestrator**

* Combines everything
* Good for dashboards or future automation

---

## 🛠 **Development Guidelines**

### Keep these in sync:

* `a0, a1, a2, a3, k`
* EMA lengths: `40 / 105 / 170`
* Thresholds: `weakThresh = 3.0`, `strongThresh = 8.0`
* fmTrendStrength (currently **0.45**)
* fmLookback (current chosen value)

When you update any of these in one script, update them in the other.

---

## 📌 **Recommended Usage**

For live trading:

### **Use only:**

* `flush-indicator.pine`
* `flush-momentum.pine` (visual only if desired)

For research:

* Compare Strict vs Relaxed
* Use orchestrator to test blended conditions
* Use srchulo-momentum to prototype momentum variants

---

## 🙏 **Notes & Contributions**

This is a private/experimental repo for Adam Hopkins' (srchulo's) personal trading tools.
