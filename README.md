# Machine-Learning-in-Ecology-R-Workshop-
Workshop materials for the SIG Aquatic Ecosystems in the Data Age event.Status: Materials will be uploaded by 24th Feb . 
Prerequisites: R 4.2+, RStudio, packages: tidyverse, randomForest, caret, vegan, ggplot2, pdp Check back soon for datasets and scripts.


# Machine Learning in Ecology – R Workshop

Workshop materials for the **Aquatic Ecosystems in the Data Age** conference.

Facilitated by **Arron Watson** (Environment Agency & University of Birmingham)

---

## 📦 Status

**Materials will be uploaded by 24th Feb**

This repository will include:
- R scripts for hands-on exercises
- Example datasets (macroinvertebrate communities + environmental covariates)
- Installation instructions and package requirements

---

## 🔧 Prerequisites

Participants should have:
- Working knowledge of R and RStudio
- Familiarity with data import and basic manipulation (`tidyverse` or base R)
- Understanding of foundational statistical concepts (regression, classification)

**No prior machine learning experience required.**

---

## 📋 Required Setup

Please ensure you have installed before the workshop:

- **R** (version 4.2 or later)
- **RStudio** (latest stable release)
- **R packages:**
```r
  install.packages(c("tidyverse", "randomForest", "caret", "vegan", "ggplot2", "pdp"))
```
# watch this on how to install packages - https://www.youtube.com/watch?v=u1r5XTqrCTQ 


# watch this on how to get coding - https://youtu.be/FY8BISK5DpM?si=eSWn-RzrfOP-rsPR



# Watch this to understand how random forest works in r - https://www.youtube.com/watch?v=6EXPYzbfLCE&t=1s 


################################################################################################ 


# Workshop outputs #


## 1) Constructing a RF model ##
* there are a lot of different things to consider but here is a demonstration of a model with bootsrapping *

################################################################################
#################### ROBUST RANDOM FOREST MODEL STRUCTURE ######################
################################################################################

### Set seed for reproducibility
set.seed(123)

### Build robust Random Forest model
rf_model_robust <- randomForest(
  formula = rf_formula,                    # Your response ~ predictors
  data = site_summary_complete,            # Training data
  
  ### ENSEMBLE PARAMETERS
  ntree = 1000,                            # Number of trees (500-2000 typical)
  
  ### BOOTSTRAP PARAMETERS  
  replace = TRUE,                          # Bootstrap sampling (default)
  sampsize = floor(0.600 * nrow(site_summary_complete)),  # ~60% per tree (or higher)
  
  ### VARIABLE SELECTION
  mtry = floor(sqrt(ncol(site_summary_complete) - 1)),  # Variables per split
  
  ### NODE SPLITTING
  nodesize = 5,                            # Min observations in terminal nodes, play around with this.
  maxnodes = NULL,                         # No limit on tree depth
  
  ### MODEL EVALUATION
  importance = TRUE,                       # Calculate variable importance
  keep.forest = TRUE,                      # Save the forest (for predictions)
  
  ### ERROR ESTIMATION
  do.trace = FALSE                         # Don't print progress
)

### Display model summary
print(rf_model_robust)

## Key robust features:
## 1. ntree = 1000: Large ensemble for stable predictions 
## 2. replace = TRUE: Standard bootstrap for tree diversity
## 3. mtry controls randomness: sqrt(p) balances bias-variance
## 4. nodesize = 5: Prevents overfitting to noise
## 5. OOB error: Built-in cross-validation

---

## 📧 Contact

Questions? Reach out via the conference organisers or open an issue in this repository.

--- arron.watson@environment-agency.gov.uk

**Check back soon for datasets and scripts!**
