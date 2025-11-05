# The Long-Run Climate Dynamics

A comprehensive Bayesian MCMC modeling framework for understanding climate dynamics across multiple timescales, from paleoclimate orbital cycles (~800,000 years) to modern anthropogenic forcing and future climate projections through 2100.

## 📋 Project Overview

This project bridges deep-time paleoclimate dynamics with modern climate science by:
- **Modeling glacial-interglacial cycles** using Milankovitch orbital parameters and greenhouse gas forcing over 800,000 years
- **Calibrating paleoclimate models** against instrumental data (1755-2017) to understand the transition from natural to anthropogenic forcing
- **Forecasting future climate trajectories** under seven IPCC-based scenarios through 2100

The analysis demonstrates that while orbital cycles (eccentricity, obliquity, precession) set the natural baseline for climate variability, anthropogenic greenhouse gas emissions now dominate climate forcing.

## 🎯 Key Results

### Long-term Paleoclimate Component
- **Model Architecture**: Sinusoidal decomposition of temperature variations into five Milankovitch cycles (19, 23, 29, 41, 100 kyr periods) plus CO₂ and CH₄ radiative forcing
- **Statistical Validation**: Likelihood ratio tests show greenhouse gases significantly improve model fit (log-likelihood: -669.8 [orbital only] → -296.5 [full model], p ≪ 0.001)
- **Spectral Analysis**: Periodogram and wavelet analyses confirm the presence of all major Milankovitch frequencies in the paleoclimate record

### Short-term Forecasting Component
- **Calibration Performance**: R² = 0.739, RMSE = 0.29°C on 1755-2017 instrumental period
- **Optimized Coefficients**: CO₂ (β=3.27), CH₄ (β=-0.61), Aerosols (β=0.45), Albedo (β=1.00)
- **Scenario Analysis**: Seven IPCC-based pathways show divergent futures, with aggressive mitigation limiting warming while "Current Policies" scenarios project continued temperature rise

## 🧮 Mathematical Framework

### Long-term Paleoclimate Model

The full model decomposes temperature anomalies into orbital cycles and greenhouse gas forcing:

```math
ΔT = Σᵢ₌₁⁵ aᵢ sin(2πt/Mᵢ) + b_CO₂ f_CO₂(t) + b_CH₄ f_CH₄(t) + T₀
```

Where:
- **Mᵢ ∈ {19, 23, 29, 41, 100} kyr**: Milankovitch cycle periods
  - 19, 23 kyr: Precession cycles
  - 29 kyr: Nonlinear climate response
  - 41 kyr: Obliquity cycle
  - 100 kyr: Eccentricity cycle
- **f_CO₂(t), f_CH₄(t)**: Hansen's radiative forcing functions
- **aᵢ, b_CO₂, b_CH₄, T₀**: Parameters estimated via MCMC

### Bayesian MCMC Inference

Log-likelihood function for parameter estimation:

```math
ln ℒ = -½ Σⱼ₌₁ⁿ [(Tⱼ - T_model(tⱼ))/σⱼ]²
```

Parameters sampled using the `emcee` ensemble sampler:
- **50 walkers** × **1000 iterations** with 200-step burn-in
- Convergence assessed via Gelman-Rubin R-hat statistic
- Initial guesses optimized using L-BFGS-B

### Short-term Forecasting Model

Combines orbital baseline with anthropogenic forcing:

```math
T(t) = T_Mil(t) + β_CO₂ f_CO₂(t) + β_CH₄ f_CH₄(t) + β_aer F_aer(t) + β_alb Δα(t)
```

Where:
- **T_Mil(t)**: Milankovitch baseline (fixed parameters from MCMC)
- **F_aer(t)**: Aerosol forcing (SAOD × -6.5 W/m²)
- **Δα(t)**: Albedo changes from land use
- **β coefficients**: Optimized via least-squares regression

## 📊 Data Sources

### Paleoclimate Data (Located in `data/`)
- **Vostok-Data.csv**: Ice core data including CO₂, CH₄, and temperature reconstructions over ~800 kyr
- **Ecc_Obl_Prec.csv**: Milankovitch orbital parameters (Laskar et al. 2004)
- **LR04stack.csv**: Benthic δ¹⁸O stack for ice volume proxy (Lisiecki & Raymo 2005)
- **Snyder 2016**: Global average surface temperature (GAST) reconstruction

### Modern Instrumental Data
- **greenhouse_gases.csv**: CO₂ (NOAA), CH₄/N₂O (AGAGE) concentrations 1755-2017
- **Temperature Data**: Berkeley Earth surface temperature anomalies

### Future Scenarios
- **IAM_Data.csv**: GCAM 6.0 NGFS model projections for seven scenarios:
  - Below 2°C
  - Current Policies
  - Net Zero 2050
  - Delayed Transition
  - Fragmented World
  - Low Demand
  - Nationally Determined Contributions (NDCs)

## 🚀 Getting Started

### Prerequisites

```bash
# Python 3.7+
pip install numpy pandas matplotlib scipy emcee corner PyWavelets scikit-learn tabulate jupyter
```

### Running the Analysis

1. **Clone the repository**:
```bash
git clone https://github.com/yourusername/Long-Run-Climate-Dynamics.git
cd Long-Run-Climate-Dynamics
```

2. **Launch Jupyter Notebook**:
```bash
jupyter notebook MCMC_Model.ipynb
```

3. **Execute cells sequentially** or run all cells to reproduce the complete analysis

### Notebook Structure

The notebook is organized into the following sections:

1. **Data Loading & Preprocessing** (Cells 1-7)
   - Import libraries
   - Load and clean paleoclimate datasets
   - Merge orbital parameters with temperature/GHG data

2. **Spectral Analysis** (Cells 8-18)
   - Periodogram analysis of orbital parameters
   - δ¹⁸O isotope analysis
   - Wavelet power spectrum visualization

3. **MCMC Model Setup** (Cells 19-29)
   - Define Milankovitch sinusoidal model
   - Implement log-likelihood and prior functions
   - Run MCMC sampling with `emcee`

4. **Model Comparison** (Cells 30-58)
   - Milankovitch-only model
   - Milankovitch + CO₂
   - Milankovitch + CO₂ + CH₄
   - Model without 29 kyr cycle
   - Likelihood ratio tests and goodness-of-fit statistics

5. **Short-term Calibration** (Cells 59-79)
   - Load modern instrumental data (1755-2017)
   - Regression analysis incorporating aerosols and albedo
   - Model validation (R², RMSE)

6. **Future Projections** (Cells 80+)
   - IAM scenario analysis
   - Temperature forecasts through 2100
   - Uncertainty quantification

## 📈 Key Visualizations

The notebook generates numerous publication-quality figures:

### Spectral Analysis
- **Periodograms**: Power spectral density plots identifying dominant frequencies
- **Wavelet Spectra**: Time-frequency heatmaps showing evolving periodicity

### Model Fits
- **Corner Plots**: Posterior distributions and parameter correlations
- **Time Series Comparisons**: Model predictions vs. observations with uncertainty bands

### Future Scenarios
- **Scenario Trajectories**: Seven warming pathways with 95% confidence intervals
- **Comparative Panels**: Side-by-side scenario visualizations

## 🔬 Methodology Highlights

### Time Series Analysis
- **Fourier Decomposition**: Periodogram with Blackman windowing to identify orbital frequencies
- **Continuous Wavelet Transform**: Complex Morlet wavelets with Gaussian smoothing for time-frequency analysis

### Bayesian Inference
- **Ensemble MCMC**: Affine-invariant ensemble sampling via `emcee`
- **Priors**: Informed priors based on known Milankovitch periods and observed temperature ranges
- **Convergence Diagnostics**: Gelman-Rubin R-hat, trace plots, autocorrelation analysis

### Model Selection
- **Likelihood Ratio Tests**: Nested model comparisons using χ² distribution
- **Information Criteria**: Comparison of R², RMSE, and log-likelihood metrics

### Radiative Forcing Functions
- **Hansen CO₂ Formula**: Logarithmic forcing function accounting for CO₂ saturation
- **Hansen CH₄ Formula**: Complex formula including N₂O interactions and overlapping absorption bands
- **Aerosol Forcing**: Stratospheric aerosol optical depth (SAOD) × -6.5 W/m²
- **Albedo Changes**: Land use impacts scaled by 10/3 factor per Hansen methodology

## 📝 Model Equations

### Hansen CO₂ Forcing
```math
f_CO₂(x) = ln(1 + 1.2x + 0.005x² + 1.4×10⁻⁶x³) - f_CO₂(x₀)
```
where x₀ = 315 ppm (pre-industrial baseline)

### Hansen CH₄ Forcing
```math
g(x,y) = 0.394x^0.66 + 0.16x·exp(-1.6x) / (1 + 0.169x^0.62)
       + 1.556·ln(1 + y^0.77(109.8 + 3.5y)/(100 + 0.14y²))
       - 0.014·ln(1 + 0.636(xy)^0.75 + 0.007x(xy)^1.52)
```
```math
f_CH₄(x) = g(x, y₀) - g(x₀, y₀)
```
where x₀ = 0.720 ppm, y₀ = 0.270 ppb (pre-industrial baselines)

## 🎓 Scientific Context

This project addresses fundamental questions in paleoclimate and climate science:

### The Milankovitch Hypothesis
- **Question**: Do orbital variations control ice ages?
- **Findings**: Spectral analysis confirms all predicted Milankovitch frequencies present in temperature records
- **Insight**: Orbital forcing provides the "pacemaker" but requires amplification by CO₂/CH₄ feedbacks

### Greenhouse Gas Feedbacks
- **Question**: Are CO₂ and CH₄ drivers or merely tracers of temperature?
- **Findings**: Inclusion of GHG forcing improves model fit by >750 log-likelihood units (p ≪ 0.001)
- **Insight**: Greenhouse gases act as critical amplifiers of orbital forcing

### Natural vs. Anthropogenic Forcing
- **Question**: How do human emissions compare to natural variability?
- **Findings**: Anthropogenic forcing now dominates; modern warming rate exceeds any in 800 kyr record
- **Insight**: Current trajectory unprecedented in timescale and magnitude

## 🔮 Future Scenarios: Key Findings

### Below 2°C / Net Zero 2050
- Aggressive emissions reductions limit warming to <2°C above pre-industrial
- Requires rapid decarbonization and negative emissions technologies
- Peak warming mid-century followed by stabilization

### Current Policies
- Business-as-usual trajectory leads to ~3-4°C warming by 2100
- Exceeds natural glacial-interglacial range (~8°C) in rate and magnitude
- High uncertainty in feedback mechanisms at such warming levels

### Delayed Transition / Fragmented World
- Policy inaction or fragmented responses lead to 2.5-3.5°C warming
- Demonstrates critical importance of near-term emissions reductions
- "Lock-in" effects make later mitigation costlier and less effective

## 🤝 Contributing

Contributions are welcome! Potential areas for expansion:

- **Additional Forcings**: Include solar variability, volcanic aerosols
- **Spatial Resolution**: Extend to regional climate patterns
- **Tipping Points**: Incorporate non-linear threshold dynamics
- **Economic Impacts**: Link temperature scenarios to damage functions
- **Alternative Models**: Compare with Earth System Models (ESMs)

## 📚 References

### Key Scientific Papers
1. **Laskar, J. et al. (2004)**: "A long-term numerical solution for the insolation quantities of the Earth." *A&A*, 428, 261-285.
2. **Lisiecki, L.E. & Raymo, M.E. (2005)**: "A Pliocene-Pleistocene stack of 57 globally distributed benthic δ¹⁸O records." *Paleoceanography*, 20, PA1003.
3. **Hansen, J. et al. (1988)**: "Global climate changes as forecast by Goddard Institute for Space Studies." *JGR*, 93(D8), 9341-9364.
4. **Snyder, C.W. (2016)**: "Evolution of global temperature over the past two million years." *Nature*, 538, 226-228.

### Data Sources
- **NOAA**: Global Monitoring Laboratory (CO₂ data)
- **AGAGE**: Advanced Global Atmospheric Gases Experiment (CH₄, N₂O)
- **Berkeley Earth**: Land/Ocean Temperature Record
- **GCAM 6.0 NGFS**: Integrated Assessment Model scenarios

### Software Libraries
- **emcee**: Foreman-Mackey et al. (2013). "emcee: The MCMC Hammer." *PASP*, 125(925), 306.
- **PyWavelets**: Lee et al. (2019). Wavelet transform library for Python.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👤 Author

**Arturo Favara**  
Research Intern, Center for Research Economics & Statistics (Paris, FRA)  
March 2024 - June 2024

## 🙏 Acknowledgments

- Center for Research Economics & Statistics (CREST) for research support
- IPCC and NGFS for scenario data
- Open-source scientific Python community

## 📧 Contact

For questions, suggestions, or collaborations, please open an issue on GitHub or contact the author directly.

---

*"The ice ages are a supreme example of a global environmental crisis. They remind us that Earth's climate is sensitive to small changes in forcing, and that feedbacks can amplify those changes dramatically."* - James Hansen

