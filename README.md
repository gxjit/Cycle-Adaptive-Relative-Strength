# Cycle Adaptive Relative Strength (CARS)

The **Cycle Adaptive Relative Strength (CARS)** is a highly responsive, dynamically adjusting momentum oscillator written in Pine Script v6.

Traditional relative strength indicators (like the classic RSI) rely on a fixed lookback period—most commonly 14 bars. While effective in stationary markets, a static lookback forces the indicator to lag during rapid trend transitions and generate false signals during cyclical consolidations.

CARS solves this by continuously measuring the market's live **Dominant Cycle** and using it to adjust its internal smoothing filters on the fly. By replacing traditional smoothing with a 3-Pole SuperSmoother Filter and synchronizing it with the market's natural heartbeat, CARS provides a low-lag, highly accurate reading of bullish versus bearish momentum.

---

## Methodology: Under the Hood

The indicator is built upon four sequential mathematical and signal processing components:

### 1. Dominant Cycle Estimation

The engine of CARS utilizes a Hilbert Transform to continuously evaluate price action and estimate the market's current dominant cycle period. This raw cycle length is clamped between user-defined minimum and maximum limits (defaulting to 8 and 48) to maintain structural stability and ignore erratic data spikes.

### 2. Adaptive Cycle Scaling

Once the Dominant Cycle is identified, it is multiplied by a user-defined `Cycle Multiplier`. This allows the trader to tune the indicator's reactivity. A multiplier of 1.0 synchronizes the indicator directly with the dominant cycle, while a multiplier of 0.5 (half-cycle) makes it incredibly fast, ideal for catching early turning points.

### 3. Separated Gain/Loss SuperSmoothing

Unlike standard RSI, which relies on Wilder's Moving Average (RMA), CARS calculates the bar-to-bar price changes and separates them into absolute Gains and absolute Losses. These raw values are then passed through a **3-Pole SuperSmoother Filter (SSF3)** using the adapted cycle length. The SSF3 filter efficiently strips out high-frequency market noise (aliasing) with low lag.

### 4. Normalized Relative Strength

Finally, the relative strength is calculated as the normalized difference between the smoothed gains and smoothed losses. The mathematical structure is expressed as: (avgGain - avgLoss) / (avgGain + avgLoss)

This bounds the oscillator tightly between -1.0 and +1.0 (plotted around a zero line), making it universally readable across any asset class or timeframe.

---

## For Traders: How to Read & Use

CARS plots as a sleek, color-changing oscillator line below your main price chart, fluctuating around a central Zero Line. Because its underlying lengths adapt to market volatility, it is significantly more decisive than a standard RSI.

**Trend Regimes (Zero-Line Crossovers)**

* **Bullish Regime:** When CARS crosses above the Zero Line (value > 0), short-term buying pressure has mathematically overtaken selling pressure. The plot shifts to the designated **Up** color, confirming a bullish environment.
* **Bearish Regime:** When CARS drops below the Zero Line (value < 0), selling pressure is dominant. The plot shifts to the designated **Down** color, signaling structural weakness.

**Momentum Velocity**

* The slope and amplitude of CARS reveal the velocity of the trend. A steepening curve indicates accelerating momentum, whereas a flattening curve near the extremes suggests the current cyclical push is exhausting and a mean-reversion event may be imminent.

**Divergences**

* Because CARS is perfectly in phase with the market's dominant cycle, classical divergences are highly potent. If price makes a new high but CARS registers a lower high, it strongly signals that the underlying cyclical momentum is failing, often preceding a reversal.

---

## ⚙️ Settings Guide

| Setting | Description | Recommendation |
| --- | --- | --- |
| **Source** | The price data used for the calculations. | **Close** is default, but **hl2** (Median Price) is highly recommended for DSP indicators to capture the true midpoint of price action. |
| **Min Dominant Cycle** | The lowest cycle length the estimator is allowed to return. | **8** is the default. Lower values increase the algorithm's reactivity to noise. |
| **Max Dominant Cycle** | The highest cycle length the estimator is allowed to return. | **48** is the default, capturing most macro cyclical movements before they break down into trends. |
| **Cycle Multiplier** | A fractional scaling factor applied to the measured cycle. | **1.0** tracks the full cycle. Use **0.5** for aggressive, rapid signals (half-cycle alignment). |
| **Colors (Up/Down)** | Controls the visual rendering of the oscillator line. | Defaulted to visually pleasing muted tones (Grey/Blue and Brown). Adjust to fit your chart template. |

---

## 📜 Acknowledgements & Credits

* **John F. Ehlers:** The foundation of this indicator heavily relies on Ehlers' pioneering work in applying Digital Signal Processing (DSP) to financial markets. The Hilbert Transform homodyne discriminator for cycle measurement and the SuperSmoother filter are direct applications of his research.
* **J. Welles Wilder Jr.:** For the foundational concept of mathematically separating average gains and average losses to measure relative strength (RSI). CARS serves as a modern, DSP-driven evolution of his original concept.
