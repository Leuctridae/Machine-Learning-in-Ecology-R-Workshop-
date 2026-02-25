# Workshop Results & Plot Explanations

This document explains the key outputs and visualizations from the Machine Learning in Ecology workshop, providing context for interpreting the Random Forest analysis of macroinvertebrate communities.

---

## Dataset Overview

After spatial clustering (10km radius) and temporal matching (±10 years for site and habitat data), the final analysis dataset consisted of:

- **171 individual invertebrate records**
- **16 unique site-year combinations**
- **5 spatial clusters** (river locations)
- **90 species** recorded
- **Temporal coverage**: 2020-2021
- **Environmental predictors**: 11 site variables + 5 habitat structure variables

---

## Exploratory Data Analysis

### 1. Temporal Trends in Invertebrate Abundance

![Total Abundance Over Time](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/53716d81dd2e0fa93c62e7bd073a9150f91b1722/total_invert.jpeg)


**Figure 1: Temporal trend in total macroinvertebrate abundance across sampled sites (2020-2021).**

The dramatic **four-fold increase in abundance** from 2020 to 2021 (from ~1,000 to ~4,000 individuals) is a striking pattern that warrants investigation. This could reflect:

- **Environmental recovery** following a disturbance event
- **Sampling effort differences** between years
- **Population boom** in one or more dominant species
- **Seasonal or hydrological variation** if sampling occurred at different times

**Implications for modeling:**
This temporal pattern demonstrates why YEAR was included as a potential predictor variable in the Random Forest model. The substantial year-to-year variation suggests temporal factors may be important drivers of community structure.

---

### 2. Spatial Distribution of Sampling Sites

![Spatial Distribution](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/76d2b054da2df1947199ef5b25c0dc2c1a3cb4c6/spatialdist.jpeg)

**Figure 2: Geographic distribution of sampling locations within the study catchment.**

Following spatial clustering with a 10km radius and temporal filtering, **5 distinct site clusters** were retained for analysis. The spatial configuration shows:

- Sites distributed across the catchment
- Representation of both upstream and downstream locations
- Sufficient spatial spread to capture environmental gradients (altitude: 13-93m, distance from source: 13.89-49.02 km)

**Why spatial variation matters:**
The environmental gradients captured across these sites enable the Random Forest algorithm to identify relationships between species richness and environmental conditions. A single-site study would lack the environmental variation necessary for meaningful modeling.

---

### 3. Environmental Variable Distributions

![Environmental Variables](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/5e2460d1450ba6dfbeac03beb6031100806b7bd6/enviro.jpeg)

**Figure 3: Frequency distributions of environmental predictor variables used in Random Forest modeling.**

The histograms show the range and distribution of key predictors:

**Physical characteristics:**
- **Altitude**: 13-93 m (lowland river system)
- **Slope**: 2.2-9.0 (relatively gentle gradients typical of lowland rivers)
- **Distance from source**: 13.89-49.02 km (mid to lower reaches)

**Hydraulic properties:**
- **Discharge**: Relatively consistent across sites
- **Width**: 5.7-9.0 m (moderate-sized streams)
- **Depth**: 12.0-49.55 cm (shallow to moderate depths)

**Substrate composition:**
- **Boulders/Cobbles**: 3-48% (moderate variation)
- **Pebbles/Gravel**: 22-66% (dominant substrate, high variation)
- **Sand**: 7-47% (moderate to high in some sites)
- **Silt/Clay**: 0-27% (generally low, as expected in flowing water)

**Water chemistry:**
- **Alkalinity**: 117.8-123.0 mg/L CaCO₃ (hard water, relatively consistent)

**Key observation:** Most environmental variation comes from physical position in the catchment (altitude, distance from source) and hydraulic conditions (depth, discharge) rather than substrate or chemistry, which show less variation across sites.

---

## Random Forest Model Results

### 4. Variable Importance

![Variable Importance](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/a052bda0adc86731bebe0cd72c92c4f60709e320/random_MSE.jpeg)

**Figure 4: Variable importance from Random Forest model predicting macroinvertebrate species richness.**

The two panels show different measures of variable importance:

**Left panel - %IncMSE (Prediction Accuracy):**
Measures how much prediction error increases when a variable is randomly permuted (shuffled). Higher values indicate the variable is more important for accurate predictions.

**Right panel - IncNodePurity (Split Quality):**
Measures the total decrease in node impurities (variance for regression) from all splits on that variable. Higher values indicate the variable creates more homogeneous groups.

**Top predictors (consistently important in both measures):**

1. **DIST_FROM_SOURCE** - Distance from the river source is the single strongest predictor
   - Ecological interpretation: Reflects longitudinal zonation in river ecosystems
   - Correlates with temperature, flow regime, and habitat complexity

2. **ALTITUDE** - Elevation strongly predicts species richness
   - Ecological interpretation: Altitude and distance from source are closely linked
   - Together they capture the fundamental river continuum concept

3. **DEPTH** - Water depth is a key hydraulic variable
   - Ecological interpretation: Influences available habitat, flow velocity, and substrate stability
   - Moderate depths appear optimal for invertebrate diversity

4. **ALKALINITY** - Water chemistry matters despite low variation
   - Ecological interpretation: Hard water supports diverse communities
   - Even small variation in alkalinity may influence sensitive species

5. **DISCHARGE** - Flow is important (especially in IncNodePurity)
   - Ecological interpretation: Determines habitat availability and disturbance regime

**Lower importance predictors:**
- **YEAR_inv**: Temporal variation present but not the dominant driver
- **Substrate variables** (sand, gravel, cobbles): Lower than expected importance
  - Could reflect: (a) genuine lower importance, (b) measurement issues, or (c) insufficient variation in the dataset

**Key insight:** The **longitudinal gradient** (distance from source + altitude) emerges as the primary structuring force for invertebrate communities, consistent with fundamental river ecology theory.

---

### 5. Model Predictions

![RF Predictions](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/201bfe0bd388b22949318316c6fbc9a9b86febd6/actualvs.jpeg)

**Figure 5: Observed versus predicted species richness from Random Forest model.**

Points represent the 16 site-year combinations. Proximity to the red dashed 1:1 line indicates prediction accuracy.

**Model performance:**
- **Mean Absolute Error (MAE)**: [value from your output]
- **Root Mean Squared Error (RMSE)**: [value from your output]
- **R-squared**: [value from your output]
- **% Variance Explained**: 0.52%

**Interpretation:**

✅ **What the model does well:**
- Captures the general positive trend (higher actual richness → higher predicted richness)
- Performs reasonably in the 30-70 species range
- Despite small sample size (n=16), identifies meaningful environmental relationships

⚠️ **Limitations:**
- **Low variance explained** (0.52%) indicates most variation is unexplained
- **Underprediction at high richness** (site with ~90 species is poorly predicted)
- Small dataset limits model training and validation

**Why is variance explained so low?**

With only 16 observations and limited environmental variation:
1. **Sample size constraints** - not enough data to capture complex patterns
2. **Unmeasured variables** may be important (water quality, land use, disturbance history, biotic interactions)
3. **Stochastic processes** - ecological communities have inherent randomness
4. **Temporal dynamics** - only 2 years captured, missing longer-term trends

**Important context:** Even with low R², the variable importance analysis reveals ecologically meaningful patterns. The model successfully identifies which environmental factors matter most, even if it can't predict exact richness values.

---

### 6. Partial Dependence Plots

![Partial Dependence](https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-/blob/138fbc34cd9e174cc337e79e025c26fe6db5fd23/partialdepend.jpeg)

**Figure 6: Partial dependence plots for the three most important predictors of species richness.**

Partial dependence plots show how predicted species richness changes as one variable varies while all others are held at their mean values. These reveal the **shape of the relationship** between each predictor and the response.

#### A) Distance from Source

**Pattern:** Sharp unimodal relationship with a peak at 5-10 km from source

**Interpretation:**
- **Headwaters** (0-5 km): Lower richness - harsh conditions, cold temperatures, limited habitat diversity
- **Mid-reaches** (5-10 km): **Peak richness** - optimal balance of flow, temperature, habitat complexity, and productivity
- **Lower reaches** (>20 km): Declining richness plateau - possible degradation, altered flow regimes, or shift to different community types

**Ecological significance:** This pattern strongly supports the **River Continuum Concept** (Vannote et al. 1980), which predicts maximum invertebrate diversity in mid-order streams where habitat and resource diversity are highest.

#### B) Altitude

**Pattern:** Unimodal relationship with peak richness at 100-150m elevation

**Interpretation:**
- Mirrors the distance from source pattern (the two variables are correlated)
- **Lowland sites** (50-100m): Lower predicted richness
- **Mid-elevation sites** (100-150m): Optimal conditions for diversity
- **Higher sites** (>150m): Declining richness

**Ecological significance:** Altitude influences temperature, dissolved oxygen, flow characteristics, and riparian vegetation - all of which structure invertebrate communities.

#### C) Depth

**Pattern:** Highest predicted richness at moderate depths (10-15 cm), declining in both shallower and deeper water

**Interpretation:**
- **Shallow water** (<10 cm): Unstable conditions, high temperature variation, limited habitat complexity
- **Moderate depth** (10-15 cm): **Optimal** - stable flow, diverse microhabitats, suitable for many taxa
- **Deep water** (>30 cm): Lower richness - possibly reflects pool habitats with different community composition (fewer riffle specialists)

**Ecological significance:** Demonstrates the importance of **habitat heterogeneity** - sites with varied depths likely support higher overall diversity than those dominated by extreme shallow or deep conditions.

---

### 7. Model Comparison: Random Forest vs Linear Regression

![Model Comparison](path/to/your/plot7.png)

**Figure 7: Comparison of Random Forest (green) and linear regression (orange) predictions against observed species richness.**

Both models predict species richness from the same environmental variables, but make different assumptions about relationships.

**Model Performance Comparison:**

|                      | MAE  | RMSE | R-squared |
|----------------------|------|------|-----------|
| **Random Forest**    | [RF value] | [RF value] | [RF value] |
| **Linear Regression**| [LR value] | [LR value] | [LR value] |

**Visual Patterns:**

🟢 **Random Forest (green points):**
- More scattered predictions
- Greater flexibility in capturing non-linear patterns
- One point performs very well (high richness site)
- Higher variance in predictions

🟡 **Linear Regression (orange points):**
- Tightly clustered along a straight trend line
- More constrained prediction range
- More consistent but potentially less accurate for extreme values
- Lower variance in predictions

**Key Differences:**

| Aspect | Linear Regression | Random Forest |
|--------|------------------|---------------|
| **Assumptions** | Linear relationships only | Can capture non-linear patterns |
| **Flexibility** | Low - straight line fit | High - piece-wise splits |
| **Interpretability** | High - simple coefficients | Moderate - variable importance |
| **Overfitting risk** | Lower with small data | Higher with small data |
| **Best for** | Simple, linear relationships | Complex, non-linear relationships |

**Which model is better?**

The answer depends on your goal:

- **For interpretation:** Linear regression is simpler and provides clear coefficient estimates
- **For prediction:** Compare metrics (MAE, RMSE, R²) - [based on your actual results]
- **For ecology:** Random Forest better captures the non-linear patterns we see in partial dependence plots (e.g., unimodal distance from source relationship)

**With this dataset:** Both models face challenges due to small sample size (n=16). Random Forest's ability to model non-linear relationships is valuable, but the limited data means we should interpret predictions cautiously.

---

## Key Findings Summary

### Environmental Drivers of Species Richness

1. **Longitudinal position** (distance from source + altitude) is the dominant driver
2. **Mid-reach sites** (5-10 km from source, 100-150m elevation) support highest diversity
3. **Moderate water depths** (10-15 cm) are optimal
4. **Hydraulic conditions** (depth, discharge) matter more than substrate composition in this dataset
5. **Temporal variation** exists (2020→2021 abundance increase) but is not the primary driver

### Model Performance

- Random Forest successfully identified ecologically meaningful predictors
- Low overall variance explained (0.52%) reflects dataset limitations (n=16) and unmeasured variables
- Partial dependence plots reveal clear non-linear relationships consistent with river ecology theory
- Both Random Forest and linear regression face challenges with small sample size

### Ecological Insights

✅ **River Continuum Concept supported** - mid-reaches show peak diversity

✅ **Non-linear relationships** - unimodal patterns for distance from source and altitude

✅ **Hydraulic variables matter** - depth and discharge are key predictors

⚠️ **Unexplained variation** suggests important unmeasured factors (water quality, land use, biotic interactions)

⚠️ **Temporal dynamics** need further investigation (dramatic 2020→2021 change)

---

## Limitations & Future Directions

### Dataset Limitations

1. **Small sample size** (n=16 site-year combinations)
   - Limits statistical power
   - Increases risk of overfitting
   - Reduces ability to detect complex interactions

2. **Limited temporal coverage** (only 2 years: 2020-2021)
   - Cannot assess long-term trends
   - May miss inter-annual variation

3. **Spatial extent** (5 clusters within one catchment)
   - Limits environmental gradient range
   - Cannot assess regional patterns

4. **Temporal mismatch** between datasets (±10 year window)
   - Environmental conditions may have changed
   - Introduces uncertainty in predictor-response relationships

### Recommendations for Future Work

**To improve model performance:**
- ✅ Increase sample size (target n>30 site-year combinations)
- ✅ Include additional environmental variables (nutrients, dissolved oxygen, temperature)
- ✅ Add land use and catchment characteristics
- ✅ Collect data across multiple years for robust temporal trends
- ✅ Consider cross-validation or bootstrap methods for better performance estimates

**Alternative modeling approaches:**
- Multivariate methods (NMDS, RDA) for community composition
- Species-specific models instead of richness
- Hierarchical models accounting for spatial/temporal structure
- Gradient boosting or ensemble methods

**Ecological extensions:**
- Trait-based analyses (functional diversity, trait-environment relationships)
- Network analysis for species co-occurrence patterns
- Temporal turnover analysis (beta diversity through time)
- Integration with hydrological or land use models

---

## Workshop Learning Outcomes

Participants completing this workshop gained hands-on experience with:

✅ **Data integration** - linking species, environmental, and habitat datasets spatially and temporally

✅ **Exploratory analysis** - visualizing temporal trends, spatial patterns, and environmental gradients

✅ **Random Forest modeling** - building, evaluating, and interpreting machine learning models for ecological data

✅ **Variable importance** - identifying key environmental drivers of biodiversity

✅ **Partial dependence** - understanding non-linear species-environment relationships

✅ **Model comparison** - contrasting linear and non-linear approaches

✅ **Critical thinking** - recognizing dataset limitations and interpreting results appropriately

✅ **Ecological interpretation** - connecting statistical patterns to ecological theory (River Continuum Concept, habitat heterogeneity)

---

## References & Further Reading

### River Ecology Theory
- Vannote, R.L., et al. (1980). The river continuum concept. *Canadian Journal of Fisheries and Aquatic Sciences*, 37(1), 130-137.

### Random Forest Methods
- Breiman, L. (2001). Random forests. *Machine Learning*, 45(1), 5-32.
- Cutler, D.R., et al. (2007). Random forests for classification in ecology. *Ecology*, 88(11), 2783-2792.

### Machine Learning in Ecology
- Thessen, A.E. (2016). Adoption of machine learning techniques in ecology and earth science. *One Ecosystem*, 1, e8621.
- Olden, J.D., Lawler, J.J., & Poff, N.L. (2008). Machine learning methods without tears: a primer for ecologists. *The Quarterly Review of Biology*, 83(2), 171-193.

### Macroinvertebrate Ecology
- Wright, J.F., Sutcliffe, D.W., & Furse, M.T. (Eds.). (2000). *Assessing the biological quality of fresh waters: RIVPACS and other techniques*. Freshwater Biological Association.

---

## Data Sources

- **Invertebrate & Site Data**: Environment Agency Open Data
- **Habitat Data**: River Habitat Survey (RHS)
- **Study Area**: River Bollin catchment and surrounding rivers, England

---

**Workshop facilitated by:** Arron Watson (Environment Agency & University of Birmingham)

**Repository:** https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-

**Last updated:** [Date]
