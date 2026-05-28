 Alpha 001 — Mean Reversion on Thin Volume

Date: May 2026  
Status: Submitted & Active on WorldQuant BRAIN  
Type:Mean Reversion  
Universe:USA TOP3000  

---

## The Idea

When a stock moves unusually far in one direction on a day where trading volume is thin, it's likely an overreaction — not a real, informed move. Real moves are backed by heavy volume (institutions, informed traders). Thin volume moves are noise. Noise reverts.

So the strategy is: **find stocks that moved extremely on low volume, and bet against that move.**

---

## Building the Signal Step by Step

**Step 1 — Measure how extreme the price move was**  
Used zscore of returns rather than raw returns. Raw returns aren't comparable across stocks — a 2% move means different things for different stocks. zscore measures how many standard deviations away from the average each stock moved today, making every stock comparable on the same scale.

**Step 2 — Flip for reversion**  
Multiplied by -1 to bet against the move. A stock that moved up unusually gets a short signal — we expect it to fall back.

**Step 3 — Smooth over a short lookback window**  
Single day returns are too noisy — the signal changed every day, causing excessive turnover. Smoothing returns over a short multi-day window captures the trend more stably. This reduced turnover significantly and improved Sharpe.

**Step 4 — Volume filter**  
Compared recent average volume against today's volume. When today's volume is high relative to normal, the signal weakens — the move might be real. When volume is thin, the signal strengthens — likely a fluke worth fading. Normalised cross-sectionally to avoid extreme values.

**Step 5 — Second confirmation signal**  
Added a second independent reversion signal based on recent price change to strengthen conviction. When both signals agree a stock overreacted, the combined score is stronger.

---

## Results (IS Period)

| Metric | Value | Requirement | Status |
|--------|-------|-------------|--------|
| Sharpe | 1.63 | ≥ 1.25 | ✅ |
| Fitness | 1.01 | ≥ 1.0 | ✅ |
| Turnover | 53.88% | 1%–70% | ✅ |
| Returns | 20.67% | — | — |
| Drawdown | 10.54% | — | — |

**Out-of-sample Sharpe: 2.49** — alpha performed stronger on unseen data than training data, suggesting the logic is genuine rather than overfitted.

---

## Key Learnings

- **Single day signals are too noisy.** The biggest improvement came from smoothing returns over multiple days — turnover dropped from 83% to 53% and Sharpe jumped from 0.55 to 1.63.
- **Volume confirms whether a move is real.** High volume = informed move, don't fade it. Low volume = noise, fade it.
- **Out-of-sample performance matters more than in-sample.** An alpha that does better on data it hasn't seen is more trustworthy than one that only looks good historically.
- **Fitness penalises turnover.** Every unnecessary trade costs money in the real world. Keeping turnover low is as important as keeping returns high.

---

## What I'd Improve Next

- Add industry neutralization to remove sector-wide bets
- Explore incorporating fundamental data as an additional filter
- Test different smoothing windows to find the optimal lookback
