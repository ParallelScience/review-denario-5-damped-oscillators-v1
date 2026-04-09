# **_Skepthical_** review: *Analytical Deconvolution of Noise-Induced Bias in Energy Decay Dynamics*

## Summary

The manuscript addresses a practical bias in estimating energy decay of damped harmonic oscillators from measured displacement and velocity: because energy is quadratic in “$x$” and “$v$”, additive zero-mean measurement noise yields a positive, approximately constant contribution to the estimated energy, producing an artificial late-time “energy floor” and biasing damping-rate estimates obtained by exponential fits (Sec. 1). The proposed correction estimates noise variances from a late-time window assumed to contain negligible physical motion (here $t > 15$ s), after local linear detrending, and subtracts an analytically derived constant offset $\Delta E_{\text{noise}} = \frac{1}{2} (k \sigma_x^2 + m \sigma_v^2)$ from the measured energy; negative corrected energies are clipped to zero before fitting (Sec. 2.2–2.3).

Validation on 20 simulated oscillators with known $m$, $k$, and $b$ shows that the correction removes the plateau and yields fitted damping rates in close agreement with theoretical expectations (Sec. 3.2, Table 1; Fig. 1–2). The idea is clear and potentially useful as post-processing for experiments, but several key points limit rigor and generality as written: (i) an internal inconsistency in the energy-decay model and damping-rate definition across sections, (ii) under-specified noise/measurement assumptions (especially for velocity), (iii) untested sensitivity to the late-time window, detrending, and clipping, and (iv) narrow validation without comparisons to natural baselines (e.g., fitting an exponential-with-offset model). Clarifying the modeling and adding targeted robustness checks would substantially strengthen the paper and its broader applicability claims.

## Strengths

- Clearly identifies and explains the mechanism by which squaring noisy displacement/velocity produces a nonzero mean energy offset and an artificial late-time plateau (Sec. 1).
- Proposes a simple, closed-form correction that is physically interpretable and dimensionally consistent: $\Delta E_{\text{noise}} = \frac{1}{2} (k \sigma_x^2 + m \sigma_v^2)$ (Sec. 2.2).
- Uses simulated oscillators with known parameters to enable direct comparison between fitted and theoretical damping quantities (Sec. 2.1–2.3; Sec. 3.2).
- Presents intuitive diagnostics (time-series illustration and identity/residual plots) that make the bias and its correction easy to see (Fig. 1–2).
- The overall workflow (identify plateau $\rightarrow$ estimate noise statistics $\rightarrow$ subtract bias $\rightarrow$ refit decay) is straightforward and potentially useful for experimental practitioners dealing with noise floors.

## Major issues

1.  **Inconsistent exponential decay model and damping-rate definition across the manuscript (factor-of-two mismatch).** Sec. 1 and Sec. 2.3 describe energy decay as $E(t) = E_0 \exp(-2 \gamma t)$ (and Eq. (3) appears to use $\exp(-2 \gamma_{\text{obs}} t)$), while Sec. 3.2 states fitting $E_{\text{corrected}}(t) = E(0) \exp(-\gamma_{\text{obs}} t)$. This ambiguity obscures whether $\gamma$ refers to amplitude decay or energy decay, and whether $\gamma_{\text{obs}}$ is directly comparable to $\gamma_{\text{theory}} = b/(2m)$ (Sec. 1; Sec. 2.3; Sec. 3.2).
    
    *Recommendation:* Adopt one consistent convention and apply it uniformly in Sec. 1, Sec. 2.3, Eq. (3), and Sec. 3.2. For example: start from the equation of motion $m\, \ddot{x} + b\, \dot{x} + k x = 0$ (underdamped case), note amplitude $\propto \exp(-\gamma t)$ with $\gamma = b/(2m)$, and therefore energy $\propto \exp(-2 \gamma t)$. If instead you fit $E(t) = E_0 \exp(-\lambda t)$, explicitly define $\lambda = 2\gamma$ and compare $\lambda_{\text{obs}}$ to $\lambda_{\text{theory}} = b/m$. Update all reported values/units accordingly so $\gamma_{\text{obs}}$ and $\gamma_{\text{theory}}$ are unambiguously comparable.

2.  **Noise/measurement model is under-specified, and the derivation/validity conditions for $\Delta E_{\text{noise}}$ are not stated explicitly.** The correction implicitly assumes additive, zero-mean, stationary noise and that cross-terms vanish in expectation (e.g., $\mathbb{E}[x \eta_x]=0$), but these assumptions are not formalized; correlations (including between $x$ and $v$ channels) can change the bias structure (Sec. 2.2; Sec. 3.1).
    
    *Recommendation:* In Sec. 2.2, explicitly write the measurement model (e.g., $x_m = x + \eta_x$, $v_m = v + \eta_v$) and state assumptions needed for the bias term: $\mathbb{E}[\eta] = 0$, $\operatorname{Var}[\eta]$ constant in time (stationarity over the relevant interval), and zero correlation with the signal (or at least $\mathbb{E}[x \eta_x]=0$, $\mathbb{E}[v \eta_v]=0$). Show the short expectation calculation leading to $\mathbb{E}[E_m]=\mathbb{E}[E_{\text{true}}]+\Delta E_{\text{noise}}$ in the late-time regime. State whether you assume $\operatorname{Cov}(\eta_x, \eta_v) = 0$ and comment on what changes if it is nonzero.

3.  **Velocity treatment is potentially unrealistic for experiments and may invalidate the assumed constant-bias form if $v$ is obtained by differentiating noisy displacement.** The manuscript appears to assume $v(t)$ is directly available and independently noisy, but in many settings $v$ is computed numerically from $x$, which amplifies high-frequency noise and introduces temporal correlation and $x$–$v$ noise coupling (Sec. 2.1–2.2).
    
    *Recommendation:* Clarify in Sec. 2.1–2.2 how $v$ is generated/measured in simulation: (i) independently simulated noisy velocity channel, or (ii) numerical differentiation of noisy $x$. If (i), add a limitation statement about applicability to experiments where $v$ is not directly measured. If (ii) (or to broaden applicability), include an additional validation case where $v$ is estimated from noisy $x$ (finite differences / Savitzky–Golay / filtering), report how $\sigma_v^2$ is estimated consistently, and discuss how the bias formula and detrending should be adapted under correlated/colored differentiation noise.

4.  **Key assumptions behind selecting the late-time window ($t > 15$ s) and using local linear detrending are not justified or stress-tested.** The method assumes negligible physical motion and stationary noise in that window, but there is no sensitivity analysis to cutoff choice, residual motion, drift, or colored noise; this limits confidence for real data (Sec. 2.2; Sec. 3.1; Sec. 4).
    
    *Recommendation:* In Sec. 2.2 and Sec. 3.1, justify $t > 15$ s relative to the slowest decay in the simulated set (e.g., report max time constant / remaining theoretical energy fraction at 15 s for all oscillators). Add a sensitivity study varying the late-time start and/or window length (e.g., $t > 10, 12, 15, 18$ s) and report resulting variation in $\Delta E_{\text{noise}}$ and $\gamma_{\text{obs}}$ residual statistics (mean/SD, MAE). Add at least one stress-test where the late-time segment contains (a) low-amplitude residual oscillation and/or (b) slowly drifting variance or colored noise, and summarize failure modes and practical checks (e.g., stationarity diagnostics) in Sec. 4.

5.  **Clipping negative corrected energies to zero introduces censoring/nonlinearity that can bias exponential fits, especially when many late-time points are clipped (the manuscript also hints this may explain the residual mean).** The fit protocol (time range, weighting, inclusion/exclusion of clipped points) is not described in enough detail to assess bias (Sec. 2.2; Sec. 2.3; Sec. 3.2).
    
    *Recommendation:* Make the fitting protocol explicit in Sec. 2.3: fitting interval, objective (linear-in-log vs nonlinear least squares), weights, bounds/initialization, and how points with $E_{\text{corrected}} \leq 0$ (or clipped zeros) are handled. Quantify clipping frequency across oscillators/SNR (Sec. 3.2). Add a small ablation: compare $\gamma_{\text{obs}}$ when (i) clipping+fit all points, (ii) no clipping but fit only points with $E_{\text{corrected}}>0$, and/or (iii) fit only up to a cutoff where SNR remains above a threshold. Use this to give actionable guidance for practitioners on avoiding fit bias at low SNR.

6.  **Validation scope is narrow and does not include natural baselines/competitors; this weakens the bigger-picture claim of broad experimental usefulness.** The current evaluation uses only 20 idealized simulated linear oscillators, and does not compare to simple alternatives such as subtracting the late-time mean energy floor, fitting only early-time points, or fitting an exponential-with-offset model $E(t) = E_0 \exp(-2\gamma t)+C$ (Sec. 3.1–3.2; Sec. 4).
    
    *Recommendation:* In Sec. 3.2, add baseline comparisons alongside the proposed method: (a) subtract $\operatorname{mean}(E_{\text{total}})$ over the same late-time window; (b) fit only early-time data above an SNR threshold; (c) directly fit $E_{\text{total}}(t)$ to $E_0 \exp(-2\gamma t)+C$ (or $\exp(-\lambda t)+C$) and compare bias/variance and parameter identifiability. Report the same residual metrics as Table 1 for each baseline. If feasible, broaden validation with (i) more diverse parameter ranges and SNRs, and (ii) at least one realism stress-test (e.g., mild nonlinearity, two-mode response, time-varying damping, colored/nonstationary noise). If expansion is not feasible, temper claims in Sec. 4 and clearly delimit applicability to the tested conditions.

7.  **Reproducibility is limited by missing implementation details: simulator setup (integrator, time step/sampling rate, initial conditions), parameter distributions/ranges ($m,k,b$), exact noise statistics (distribution, $\sigma_x$, $\sigma_v$, whiteness/coloredness, whether shared across oscillators), detrending algorithm details, and optimizer settings for the fit (Sec.** 2.1–2.3).
    
    *Recommendation:* Augment Sec. 2.1–2.3 (or add an appendix) with a concise but complete specification: sampling rate/time step; numerical solver; ranges/distributions for $m,k,b$ and resulting $\gamma_{\text{theory}}$; initial conditions; whether noise is added to $x/v$ before computing energy; noise distribution and correlation properties; detrending procedure (global vs sliding window, window length/overlap, edge handling); and fitting algorithm (model form, optimizer, initialization, bounds, convergence criteria). Consider adding a parameter table and stating whether code/synthetic data will be released.

## Minor issues

1.  Figure 1 caption/panels appear inconsistent: the caption references Oscillators (1, 10, 20), but the figure shows only one oscillator (or panels are missing). The figure is also very small and does not mark the late-time region used for variance estimation (Fig. 1; Sec. 2.2).
    
    *Recommendation:* Ensure all referenced panels are included and labeled (a)(b)(c), or revise the caption to match what is shown. Increase figure size and font/line thickness. Shade or otherwise annotate the late-time window (e.g., gray band for $t>15$ s) used to estimate $\sigma_x^2$ and $\sigma_v^2$ so the correction procedure is visually traceable.

2.  Figure 2 would be more informative with quantitative fit diagnostics and uncertainty. It currently claims strong correlation but does not report slope/intercept/$R^2$ (with uncertainty) and provides no uncertainty estimates for $\gamma_{\text{obs}}$; small figure size may limit readability (Fig. 2; Sec. 3.2).
    
    *Recommendation:* Overlay a regression line on the identity plot and annotate slope, intercept, and $R^2$ (preferably with confidence intervals). Add uncertainty on $\gamma_{\text{obs}}$ (fit covariance, bootstrap across time samples, or per-oscillator CI/error bars). Increase size or export as vector for legibility.

3.  SNR is referenced but not cleanly defined in Methods, and its computation is ambiguous (e.g., whether $E(0)$ is ground-truth or noise-contaminated). The relationship between SNR and accuracy/precision is not quantified beyond qualitative discussion (Sec. 3.1; Sec. 2.2–2.3).
    
    *Recommendation:* Define SNR formally once (Sec. 2.2 or Sec. 2.3), including how $E_0$ is obtained and whether $\Delta E_{\text{noise}}$ uncertainty is considered. Report the SNR range across the 20 oscillators and optionally stratify residuals by SNR bins (e.g., low/medium/high) to quantify performance dependence.

4.  The manuscript uses the term “deconvolution,” but the proposed method is primarily an analytical bias/offset subtraction rather than deconvolution in the signal-processing sense (Title/Sec. 1).
    
    *Recommendation:* Either justify the terminology explicitly (what is being deconvolved from what), or rename/reframe (e.g., “bias correction,” “noise-floor subtraction,” “analytical offset correction”) to better match the method and avoid misleading expectations.

5.  Results could be made more concrete by reporting typical magnitudes of $\Delta E_{\text{noise}}$ and the fractional energy floor relative to $E_0$, not only damping residuals (Sec. 3.1–3.2).
    
    *Recommendation:* Add a short numerical summary (mean/range) of $\Delta E_{\text{noise}}$ and the late-time plateau level across oscillators, plus representative values for the oscillators highlighted in figures. This helps readers gauge when the correction materially matters.

6.  Units/interpretation of the damping parameter: the manuscript reports residuals in rad/s, but decay rates $\gamma$ in $\exp(-\gamma t)$ are typically in s$^{-1}$. Using rad/s may confuse $\gamma$ (decay rate) with angular frequency $\omega$ (Sec. 3.2; Table 1).
    
    *Recommendation:* Check and standardize units: if $\gamma$ is a decay rate in the exponential, report in s$^{-1}$. Reserve rad/s for angular frequencies ($\omega$). If you intentionally use rad/s, explain why and how it relates to your model definition.

7.  Claims of robustness and broad experimental applicability are somewhat stronger than supported by the current simulated-only, idealized evaluation (Sec. 4).
    
    *Recommendation:* Temper language in Sec. 4 and add a concise limitations paragraph: stationarity requirement, reliance on a true rest segment, potential issues with derived velocity, and expected failure modes under drift/colored noise or multimode/nonlinear dynamics.

8.  Related work/context is limited given that noise-floor subtraction and bias correction for squared quantities are common in measurement and signal processing (Sec. 1).
    
    *Recommendation:* Add a brief related-work paragraph in Sec. 1 positioning the method relative to (i) fitting with an offset term, (ii) truncating/thresholding fits by SNR, and (iii) more general state-space/likelihood approaches. The goal is not an exhaustive review, but clear framing of novelty and practical tradeoffs.

## Very minor issues

1.  Notation and typography are slightly inconsistent: $E_{\text{total}}(t)/E_{\text{corrected}}(t)/E(t)$, $E_0$ vs $E(0)$, inconsistent subscript formatting ($\gamma_{\text{theory}}$ vs $\gamma_{\rm theory}$), and occasional ambiguous exponential formatting (Sec. 1–3.2).
    
    *Recommendation:* Standardize notation and formatting throughout: define symbols once, use consistent subscripts, and write exponentials with clear parentheses and spacing (e.g., $\exp(-2\gamma t)$).

2.  Minor formatting/presentation issues reduce readability: stray heading formatting (e.g., a leading '\#'), unit spacing inconsistencies (e.g., “rad / s” vs “rad/s”), and figure placement that can interrupt paragraph flow (Sec. 3.2; Sec. 4).
    
    *Recommendation:* Perform a final proofread/typesetting pass to fix heading artifacts, standardize unit formatting, and adjust figure placement so captions and references read cleanly.

3.  Figure accessibility/readability could be improved (legend overlap, red/blue reliance, small fonts/lines), and exponential decay is harder to visually assess on linear scale (Fig. 1–2).
    
    *Recommendation:* Use a colorblind-safe palette and/or distinct line styles, move legends to avoid occlusion, increase font/line sizes, and consider plotting normalized energy ($E/E_0$) and/or semilog-y views or overlaying fit curves where appropriate.


## Mathematical consistency audit

This section audits **symbolic/analytic** mathematical consistency (algebra, derivations, dimensional/unit checks, definition consistency).

**Maths relevance:** light

The paper’s analytic content centers on (i) a constant energy-bias term computed from late-time variances of noisy displacement/velocity, (ii) subtracting this bias from measured energy, and (iii) fitting an exponential energy-decay model to estimate a damping rate. Most formulas are dimensionally consistent, but there is a central inconsistency in the exponential model’s decay constant (a missing factor of 2 in one section), creating ambiguity in the meaning of the reported damping rates.

### Checked items

1.  ⚠ **Noise energy bias formula** (Eq. (1), Sec. 2.2, p.3)
    
    - **Claim:** The constant energy offset induced by measurement noise is $\Delta E_{\text{noise}} = \frac{1}{2} k \sigma_x^2 + \frac{1}{2} m \sigma_v^2$.
    - **Checks:** dimensional/units, symbol/definition consistency, derivation completeness
    - **Verdict:** UNCERTAIN; confidence: medium; impact: moderate
    - **Assumptions/inputs:** Total energy computed as $E = 0.5\, k x^2 + 0.5\, m v^2$ using measured (noisy) $x$ and $v$, Late-time window has negligible true motion ($x \approx 0$, $v \approx 0$) after detrending, Noise is stationary in the window so variances are meaningful
    - **Notes:** Units check passes ($k \sigma_x^2$ and $m \sigma_v^2$ each have energy units). However, the paper does not show the expectation/expansion from noisy measurements (e.g., $\mathbb{E}[\tilde{x}^2]=\operatorname{Var}+\mathbb{E}[x]^2$) nor explicitly state assumptions needed for cross-terms to vanish and for detrending to remove any residual mean; thus the analytic derivation is not fully verifiable from the PDF alone.

2.  ✔ **Corrected energy definition** (Eq. (2), Sec. 2.2, p.3)
    
    - **Claim:** Corrected energy is obtained by subtracting the bias: $E_{\text{corrected}}(t) = E_{\text{total}}(t) - \Delta E_{\text{noise}}$.
    - **Checks:** algebra, dimensional/units, definition consistency
    - **Verdict:** PASS; confidence: high; impact: minor
    - **Assumptions/inputs:** Bias term $\Delta E_{\text{noise}}$ is constant in time, $E_{\text{total}}(t)$ and $\Delta E_{\text{noise}}$ are in the same energy units
    - **Notes:** This is a direct linear correction; algebra and units are consistent given Eq. (1).

3.  ⚠ **Non-negativity via clipping** (Sec. 2.2, p.3 (text after Eq. (2)))
    
    - **Claim:** Negative values of $E_{\text{corrected}}(t)$ are clipped to zero to enforce physical realism.
    - **Checks:** model/assumption consistency
    - **Verdict:** UNCERTAIN; confidence: medium; impact: moderate
    - **Assumptions/inputs:** Energy should be non-negative, Clipping does not invalidate subsequent exponential fitting
    - **Notes:** Clipping produces a rectified/censored signal that is not exactly exponential even if the underlying physical energy is. The paper does not specify whether clipped points are excluded from the fit or how this alteration is reconciled with the smooth exponential model in Eq. (3).

4.  ✔ **Theoretical damping rate from parameters** (Sec. 2.3, p.3)
    
    - **Claim:** The theoretical damping rate is $\gamma_{\text{theory}} = \frac{b}{2m}$.
    - **Checks:** symbol/definition consistency, dimensional/units
    - **Verdict:** PASS; confidence: high; impact: moderate
    - **Assumptions/inputs:** Underlying oscillator equation uses a linear viscous damping term with coefficient $b$, $\gamma$ denotes the amplitude-envelope decay rate (not the energy decay rate)
    - **Notes:** $b/(2m)$ has units of $1/\text{time}$ and is consistent with the later use of energy decay $\exp(-2 \gamma t)$ (energy decays at twice the amplitude rate).

5.  ✔ **Energy exponential decay model (methods)** (Eq. (3), Sec. 2.3, p.3; also Intro p.2)
    
    - **Claim:** Corrected energy follows $E(t) = E_0 \exp(-2\gamma_{\text{obs}} t)$, with $E_0$ as a fit parameter.
    - **Checks:** definition consistency, sanity/limiting case
    - **Verdict:** PASS; confidence: high; impact: critical
    - **Assumptions/inputs:** $\gamma_{\text{obs}}$ is intended to match $\gamma_{\text{theory}} = b/(2m)$, Energy decays exponentially with rate $2\gamma$ when amplitude decays with rate $\gamma$
    - **Notes:** Given $\gamma_{\text{theory}} = b/(2m)$, an energy model $\exp(-2\gamma t)$ is internally consistent with that definition of $\gamma$ as an amplitude decay rate.

6.  ✖ **Energy exponential decay model (results text inconsistency)** (Sec. 3.2, p.5 (paragraph beginning 'For each of the 20 oscillators...'))
    
    - **Claim:** The fit uses $E_{\text{corrected}}(t) = E(0)\,e^{-\gamma_{\text{obs}} t}$.
    - **Checks:** notation/definition consistency, cross-section consistency
    - **Verdict:** FAIL; confidence: high; impact: critical
    - **Assumptions/inputs:** Same $\gamma_{\text{obs}}$ as defined/used in Sec. 2.3 and Eq. (3)
    - **Notes:** This contradicts Eq. (3) and Sec. 2.3 (which use $\exp(-2\gamma_{\text{obs}} t)$). Unless $\gamma_{\text{obs}}$ is redefined here as the energy decay rate (i.e., $\gamma_{\text{obs}} := 2\gamma$), the paper uses two incompatible definitions. This ambiguity directly affects the interpretation of the reported $\gamma$ residuals.

7.  ⚠ **Residual definition** (Sec. 2.3, p.3; reiterated Sec. 3.2, p.5)
    
    - **Claim:** Residuals are defined as $\Delta \gamma = \gamma_{\text{obs}} - \gamma_{\text{theory}}$.
    - **Checks:** definition consistency, dimensional/units
    - **Verdict:** UNCERTAIN; confidence: medium; impact: moderate
    - **Assumptions/inputs:** $\gamma_{\text{obs}}$ and $\gamma_{\text{theory}}$ share the same meaning/units
    - **Notes:** As a definition, $\Delta\gamma$ is fine if $\gamma_{\text{obs}}$ and $\gamma_{\text{theory}}$ are the same quantity. Due to the factor-of-2 inconsistency in the fitted model, it is unclear whether $\gamma_{\text{obs}}$ is consistently defined as an amplitude decay rate everywhere.

8.  ✔ **SNR definition** (Sec. 3.1, p.4)
    
    - **Claim:** SNR is defined as the ratio of initial energy $E(0)$ to the noise-induced bias $\Delta E_{\text{noise}}$.
    - **Checks:** dimensional/units, definition consistency
    - **Verdict:** PASS; confidence: high; impact: minor
    - **Assumptions/inputs:** $E(0)$ and $\Delta E_{\text{noise}}$ are both energies
    - **Notes:** Energy/energy is dimensionless; definition is internally consistent.

### Limitations

- The provided PDF text does not include an explicit equation for $E_{\text{total}}(t) = 0.5\, m v(t)^2 + 0.5\, k x(t)^2$, so checks involving that expression rely on the paper’s implied standard definition.
- No detailed derivation is shown for why the late-time variance yields exactly the stated constant bias (e.g., expansion of squared noisy measurements and conditions for cross-terms to vanish), limiting verification to consistency checks rather than full proof.
- Figures and tables are not analytically audited beyond checking consistency of the associated mathematical definitions; numerical values and empirical outcomes are out of scope by instruction.


## Numerical results audit

This section audits **numerical/empirical** consistency: reported metrics, experimental design, baseline comparisons, statistical evidence, leakage risks, and reproducibility.

Five numeric/form checks were run: four passed (cross-referenced constants and a time-window containment check), and one failed due to an inconsistent exponent multiplier (2 vs 1) in the energy-decay model across sections.

### Checked items

1.  ✔ **C1_eq1_noise_bias_halves** (Page 3/6, Eq. (1): “$\Delta E_{\text{noise}} = 1/2\, k \sigma_x^2 + 1/2\, m \sigma_v^2$”)
    
    - **Claim:** The noise-induced energy bias is given as $\Delta E_{\text{noise}} = \frac{1}{2} k \sigma_x^2 + \frac{1}{2} m \sigma_v^2$ (also described as $0.5 (k \sigma_x^2 + m \sigma_v^2)$ elsewhere).
    - **Checks:** symbolic_equivalence
    - **Verdict:** PASS
    - **Notes:** Heuristic equivalence check passed (matching $0.5$ factors and presence of corresponding terms). Full CAS symbolic proof was not performed.

2.  ✔ **C2_mean_sd_repeated_across_sections** (Page 1/6 Abstract; Page 5/6 Results §3.2; Page 6/6 Table 1 and Conclusions)
    
    - **Claim:** Mean residual reported as $0.0082$ rad/s and standard deviation as $0.0197$ rad/s; these numbers appear in multiple places and should match exactly.
    - **Checks:** cross_reference_equality
    - **Verdict:** PASS
    - **Notes:** All cross-referenced occurrences of the mean residual ($0.0082$) and residual standard deviation ($0.0197$) match exactly after parsing.

3.  ✔ **C3_dataset_size_consistency** (Page 1/6 Abstract; Page 2/6 §2.1; Page 5/6 Figure 2 caption; Page 6/6 Table 1 caption; Page 6/6 Conclusions)
    
    - **Claim:** The paper repeatedly states the dataset contains 20 simulated oscillators; verify all mentions agree.
    - **Checks:** cross_reference_equality
    - **Verdict:** PASS
    - **Notes:** All checked mentions of the dataset size are consistent at $n = 20$.

4.  ✔ **C4_time_window_vs_total_duration** (Page 2/6 §2.1 (20-second interval); Page 3/6 §2.2 ($t > 15$ s window))
    
    - **Claim:** Signals are analyzed over a 20-second interval, and the late-time noise window is defined as $t > 15$ s; verify window lies within the recorded interval.
    - **Checks:** range_containment
    - **Verdict:** PASS
    - **Notes:** Containment condition holds: $15\,\mathrm{s} < 20\,\mathrm{s}$, so the late-time window exists within the stated record duration.

5.  ✖ **C5_exponent_factor_consistency_energy_decay** (Page 2/6 ($E(t)=E_0 \exp(-2\gamma t)$); Page 3/6 Eq. (3) ($E(t)=E_0 \exp(-2\gamma_{\text{obs}} t)$); Page 5/6 §3.2 text ($E_{\text{corrected}}(t)=E(0)e^{-\gamma_{\text{obs}} t}$))
    
    - **Claim:** Energy decay model exponent appears with a factor of 2 in Methods but appears without the factor 2 in Results text; flag as an internal consistency check (form mismatch).
    - **Checks:** model_form_consistency
    - **Verdict:** FAIL
    - **Notes:** Inconsistent exponent multiplier on $\gamma$ detected across statements (unique multipliers $\{2, 1\}$).

### Limitations

- Only the provided PDF text was used; no external datasets or code outputs are available to recompute statistics like mean/SD of residuals or SNR values.
- Figure-based quantitative claims cannot be verified without extracting underlying numeric data; pixel/plot reading is excluded by scope.
- Most equations are symbolic; fast checks are limited to algebraic equivalence, cross-reference consistency, and simple inequalities based on explicitly stated numbers.