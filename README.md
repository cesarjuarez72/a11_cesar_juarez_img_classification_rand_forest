## Key Methodology & Technical Highlights

1. **Deterministic Image Engineering:**
   * Downsampled all raw images to a standardized $64 \times 64$ grid using Lanczos interpolation.
   * Converted to single-channel 8-bit grayscale to eliminate ambient color bias and focus on geometric silhouettes.
   * Normalized pixel intensity values from integer $[0, 255]$ to floating-point $[0.0, 1.0]$.
   * Flattened each image into a $4,096$-dimensional numerical feature row.
   * Enforced an 80/20 stratified split ($112$ train / $28$ holdout test samples; exactly 7 test images per class).

2. **Hyperparameter Tuning (`GridSearchCV`):**
   * Evaluated 36 candidate parameter sets over 3 cross-validation folds (108 fits total) for Random Forest.
   * Optimal Random Forest parameters identified: `n_estimators: 50`, `max_depth: None`, `min_samples_split: 5`, `min_samples_leaf: 1`.
   * Evaluated 12 parameter sets over 3 folds for SVM; optimal configuration selected: `kernel: 'rbf'`, `C: 10`, `gamma: 'scale'`.

3. **Interpretability & Spatial Feature Mapping:**
   * Extracted mean decrease in Gini impurity across all 50 trees.
   * Reshaped feature importance scores back into a $64 \times 64$ 2D spatial heatmap.
   * Uncovered that the decision trees prioritized pixels along the upper perimeter and corners—leveraging background negative space to distinguish compact centered objects (watches/cameras) from wide-frame instruments (pianos/accordions).

4. **Production Inference Parity:**
   * Standalone inference function accepts image files or web URLs, replicates the exact resizing and normalization transformations, and outputs class predictions with calibrated probability distributions.
   * Validated on holdout accordion sample with 100.0% prediction certainty.
  
