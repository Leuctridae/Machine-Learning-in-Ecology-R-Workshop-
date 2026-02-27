# Workshop Homework: Extending Your Random Forest Analysis

**Machine Learning in Ecology Workshop**  
Facilitated by Arron Watson (Environment Agency & University of Birmingham)

---

## 🎯 Overview

Congratulations on completing the workshop! You've built a Random Forest model that explains 74.8% (or more in some peoples cases?) of variance in macroinvertebrate species richness - excellent work!

Now it's time to **explore further** and see how different decisions affect your results. The tasks below will help you develop critical thinking about machine learning workflows in ecology.

---

## 📚 Before You Start: Choosing the Right Approach

### **The Two Main Types of Machine Learning**

#### **1) Supervised Learning**
**What:** Learning from labeled data (known outcomes)  
**When:** You have a response variable + want to predict/classify  
**Examples in ecology:**
- Predicting species richness from environmental variables (your workshop!)
- Species distribution models
- Classifying habitat types from remote sensing

#### **2) Unsupervised Learning**
**What:** Finding patterns in unlabeled data (no known outcomes)  
**When:** Exploring data structure, finding groups, reducing dimensions  
**Examples in ecology:**
- Clustering similar communities (NMDS, k-means)
- Identifying habitat types from multivariate data
- Ordination techniques

### **🤔 Think About Your Question FIRST**

**Before choosing an algorithm, ask yourself:**

1. **Do I have a response variable, or am i unsure?**
   - YES → Supervised learning (Random Forest, regression, classification)
   - NO → Unsupervised learning (clustering, ordination)

2. **What am I trying to achieve?**
   - Predict outcomes? → Supervised
   - Discover patterns? → Unsupervised (and then predict outcomes?)
   - Understand drivers? → Supervised (with variable importance)

3. **What type of response do I have?**
   - Continuous (e.g., species count) → Regression
   - Categorical (e.g., high/medium/low diversity) → Classification
   - Multivariate (e.g., community composition) → Multivariate methods

**Remember:** The algorithm should serve your ecological question, not the other way around!

---

## 🔬 Homework Tasks

Work through these tasks to deepen your understanding. Each explores a different aspect of model building and ecological data analysis.

---

### **Task 1: Temporal Extension - Adding More Years** ⏰

**Question:** How does increasing temporal coverage affect model performance?

#### **What to do:**

1. **Extend your data download:**
   - Go back to Phase 2 (data loading)
   - Download invertebrate data from additional years from the fish environment data explorer  (e.g., 2015-2025 instead of 2020-2021)
   - https://environment.data.gov.uk/ecology/explorer/
   - Update your site and habitat data to match

2. **Re-run the analysis:**
   - Work through Phases 3-15 with the extended dataset (dont forget to change file names in the code for loading in data)
   - Note: You may need to adjust the temporal matching window in Phase 6/7 (we removed year because of the lack of data remember..)

3. **Compare results:**
   - How many observations do you have now vs. original?
   - Did % variance explained change?
   - Are the top predictors still the same?
   - Do partial dependence plots show the same patterns?

#### **Questions to consider:**

- Does more temporal data improve or worsen model performance? Why?
- Do you see temporal trends (e.g., is YEAR_inv more important)?
- Are relationships stable across years, or do they change?
- What does this tell you about temporal variation in ecological communities?

#### **Expected learning:**

✅ Understand how sample size affects model robustness  
✅ Recognize temporal trends vs. stable patterns  
✅ Consider trade-offs between data quantity and quality  

---

### **Task 2: Alternative Response Variables - Abundance vs. Richness** 📊

**Question:** Do the same environmental factors drive species abundance as drive species richness? (we have mixed taxon data, what are you going to do with it?)

#### **What to do:**

1. **Create abundance-based models:**
   - In Phase 9, change the response variable from `SPECIES_RICHNESS` to `TOTAL_ABUNDANCE_SUM`
   - Re-run Phases 10-15

2. **Create species-specific models:**
   - Pick a common species (e.g., the most abundant taxon)
   - Filter data to that species only
   - Model its abundance across sites

3. **Compare results:**
   - Which environmental variables are important for abundance?
   - Are they the same as for richness?
   - Does model performance (R²) differ?

#### **Questions to consider:**

- Why might different factors drive richness vs. abundance?
- Could a few dominant species be driving total abundance patterns?
- What ecological processes determine abundance vs. diversity?
- How would you decide which response variable to use in a real study?

#### **Expected learning:**

✅ Different ecological questions need different response variables  
✅ Richness ≠ abundance - they respond to different drivers  
✅ Species-specific models reveal individual responses  

---

### **Task 3: Adding New Predictors - Macrophyte Data** 🌱

**Question:** Does including macrophyte (aquatic plant) data improve predictions?

#### **What to do:**

1. **Obtain macrophyte data:**
   - use the macrophyte data in the zip folder but add in a new line to load it in and then more code to merge it like the other datasets)
   - Link to your dataset by grid reference and year

2. **Add macrophyte variables:**
   - In Phase 7, add macrophyte columns to your predictor list:
     ```r
     macrophyte_cols <- c("MACROPHYTE_RICHNESS?", "% MACROPHYTE_COVER?", ...)
     ```
   - Include in your model

3. **Re-run and compare:**
   - Does % variance explained increase?
   - Where do macrophyte variables rank in importance?
   - Do partial dependence plots show clear relationships?

4. **Test interactions:**
   - Do macrophytes matter more in certain river positions?
   - Combine with DIST_FROM_SOURCE to explore

#### **Questions to consider:**

- Why might macrophytes affect invertebrate communities?
- Do macrophytes improve the model, or just add noise?
- What if macrophyte data is only available for some years/sites?
- How do you balance adding predictors vs. losing observations?

#### **Expected learning:**

✅ Adding variables doesn't always improve models (curse of dimensionality)  
✅ Variable importance helps identify which new predictors matter  
✅ Data availability constraints real-world analyses  
✅ Ecological knowledge guides variable selection  

---

## 🚀 Extension Ideas (Advanced)

Once you've completed the core tasks, try these:

### **1. Cross-Validation** (an important skill to learn)
Implement k-fold cross-validation to test if your model generalizes to new data:
```r
# Split data into training and test sets
# Build model on training data
# Evaluate on test data
```

### **2. Model Tuning** (make the model robust!)
Experiment with Random Forest parameters:
- Try different `mtry` values (3, 5, 8, 11)
- Test different `ntree` (100, 500, 1000, 2000)
- Adjust `nodesize` (1, 5, 10)
- Which combination gives best performance?

### **3. Alternative Algorithms** (think about the question.. remember!)
Compare Random Forest to other methods:
- Gradient Boosting (`gbm` package)
- Support Vector Machines (`e1071` package)
- Neural Networks (`nnet` package)
- Which works best for your data?

### **4. Multivariate Approaches**
Instead of richness, model the entire community:
- Community composition (multivariate Random Forest)
- Ordination + environmental fitting
- Compare insights to your richness models

### **5. Spatial Autocorrelation** (more complex)
Test if nearby sites are more similar:
- Calculate Moran's I on residuals
- If significant, consider spatial Random Forest
- Does accounting for space change results?

---

## 📖 Resources for Learning More

### **Random Forest Methods**

**Essential Papers:**
- Breiman, L. (2001). "Random Forests" - *Machine Learning* 45: 5-32  
  [The original Random Forest paper]

- Cutler et al. (2007). "Random forests for classification in ecology" - *Ecology* 88: 2783-2792  
  [Ecological applications focus]

- Olden et al. (2008). "Machine learning methods without tears" - *Quarterly Review of Biology* 83: 171-193  
  [Gentle introduction for ecologists]

**Online Tutorials:**
- [StatQuest: Random Forests Explained](https://www.youtube.com/watch?v=J4Wdy0Wc_xQ) - Best visual explanation
- [An Introduction to Statistical Learning (free PDF)](https://www.statlearning.com/) - Chapter 8
- [Random Forest in R Tutorial](https://www.datacamp.com/tutorial/random-forests-classifier-python)

### **R Coding & Best Practices**

**R for Data Science:**
- [R for Data Science (2nd ed.)](https://r4ds.hadley.nz/) - Comprehensive R guide
- [Advanced R](https://adv-r.hadley.nz/) - For deeper understanding

**Machine Learning in R:**
- [`randomForest` package documentation](https://cran.r-project.org/package=randomForest)
- [`caret` package](https://topepo.github.io/caret/) - Unified ML interface
- [`tidymodels`](https://www.tidymodels.org/) - Modern ML workflow

**Ecological Applications:**
- [Environmental Computing](http://environmentalcomputing.net/) - Ecology-focused R tutorials
- [Methods in Ecology and Evolution](https://besjournals.onlinelibrary.wiley.com/journal/2041210x) - Journal with ML methods papers

### **Partial Dependence & Interpretation**

- Friedman, J.H. (2001). "Greedy function approximation: A gradient boosting machine" - *Annals of Statistics*
- Molnar, C. (2022). [Interpretable Machine Learning](https://christophm.github.io/interpretable-ml-book/) - Free online book
- `pdp` package vignette: `vignette("pdp")`

### **Community & Help**

- [Stack Overflow - R tag](https://stackoverflow.com/questions/tagged/r)
- [RStudio Community](https://community.rstudio.com/)
- [Cross Validated (stats Q&A)](https://stats.stackexchange.com/)
- Twitter/X: #rstats hashtag

---

## 💡 Tips for Success

### **1. Document Everything**
Keep notes on:
- What you changed
- Why you changed it
- What happened (better/worse/same)

### **2. Save Your Work**
```r
# Save model outputs
saveRDS(rf_model, "my_rf_model.rds")

# Save plots
png("variable_importance.png", width=800, height=600)
varImpPlot(rf_model)
dev.off()

# Export results
write.csv(results, "model_results.csv")
```

### **3. Compare Systematically**
Create a comparison table:

| Analysis | n obs | R² | MAE | Top 3 Predictors |
|----------|-------|-----|-----|------------------|
| Original | 50 | 0.748 | 7.07 | DIST, ALK, ALT |
| +3 years | ? | ? | ? | ? |
| Abundance | ? | ? | ? | ? |
| +Macrophytes | ? | ? | ? | ? |

### **4. Think Ecologically**
Always ask:
- Does this pattern make biological sense?
- What processes might explain this?
- How would I communicate this to a manager/decision-maker?

### **5. Embrace Failure**
Not all models will improve! Learning what **doesn't** work is as valuable as finding what does.

---

## 🎓 Assessment Criteria (If Submitting)

If you're submitting this work for assessment, you'll be evaluated on:

**Technical Skills (40%):**
- Correct implementation of modifications
- Appropriate use of R code
- Proper data handling and quality checks

**Critical Thinking (40%):**
- Thoughtful comparison of results
- Ecological interpretation of patterns
- Recognition of limitations and trade-offs

**Communication (20%):**
- Clear documentation of methods
- Effective visualization of results
- Concise summary of findings

---

## 📬 Questions & Support

**During the workshop series:**
- Ask questions in our online forum [link]
- Office hours: [dates/times]

**After the workshop:**
- Open a GitHub issue in this repository
- Email: [contact email]

**Sharing your work:**
- Fork this repository
- Add your own analysis scripts
- Share interesting findings via pull request!

---

## 🏆 Going Further

**Want to publish your analysis?**

Consider writing up your extended analysis for:
- *Methods in Ecology and Evolution* - Methods papers
- *Ecological Informatics* - Computational ecology
- *Freshwater Biology* - Macroinvertebrate studies
- *River Research and Applications* - River ecology

**Dataset publication:**
- Deposit refined datasets on [Environmental Information Data Centre](https://eidc.ac.uk/)
- Archive code on [Zenodo](https://zenodo.org/) for citability
- Share on [figshare](https://figshare.com/) or [Dryad](https://datadryad.org/)

---

## 📝 Checklist

Before you finish, make sure you've:

- [ ] Completed at least 2 of the 3 core tasks
- [ ] Documented your changes and results
- [ ] Created comparison tables/plots
- [ ] Written up your interpretations
- [ ] Considered ecological explanations
- [ ] Identified next steps for your research

---

## 🙏 Acknowledgments

Thank you for participating in this workshop! Your engagement with these tasks will deepen your understanding of machine learning in ecology and prepare you to apply these methods in your own research.

Remember: **The goal isn't to get perfect results, but to develop critical thinking about when, why, and how to use machine learning in ecological research.**

**Good luck, and happy modeling!** 🌊🐛📊

---

**Workshop Materials:** [(https://github.com/Leuctridae/Machine-Learning-in-Ecology-R-Workshop-)]  
**Citation:** Watson, A. (2026). Machine Learning in Ecology R Workshop. Environment Agency & University of Birmingham.

**Last updated:** [27/02/2026]
