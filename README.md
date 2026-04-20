ddi_technical_deep_dive:
  intro: |
    To deepen the technical understanding of the Dataset Difficulty Index (DDI), 
    we break down the "micro-mechanics" of each stressor. This adds academic rigor, 
    explaining the geometric and probabilistic consequences for model convergence.
    
  mechanics_of_structural_stress:
    - stressor: "D_imb (Probabilistic Weight)"
      description: |
        In a standard Cross-Entropy loss function, every sample contributes equally to the gradient. 
        When $D_{imb} > 0.6$, specific failures occur:
      mechanics:
        gradient_dominance: "The majority class dictates weight updates, leading to local minima that ignore minority features."
        threshold_shift: "In probabilistic models like Logistic Regression, the decision threshold (P=0.5) becomes statistically biased."

    - stressor: "D_dim (Hilbert Space Sparsity)"
      description: |
        As $D_{dim}$ approaches 1.0, the feature space volume grows exponentially while data points remain constant.
      mechanics:
        feature_sparsity: "In high-dimensional TF-IDF matrices, vectors become orthogonal, preventing the model from finding neighbors and causing overfitting."
        distance_breakdown: "The distance between nearest and farthest points becomes identical, neutralizing distance-based algorithms like KNN or SVM."

    - stressor: "D_over (Boundary Complexity)"
      description: "Based on the Global Fisher Score ($J$), measuring the discriminative power of features."
      mechanics:
        feature_hijacking: "Key tokens (e.g., 'Win', 'Free') appear in both classes with similar frequencies, weakening their signal."
        kernel_necessity: "Classes are not linearly independent; requires Kernel Tricks or Attention Mechanisms to resolve context."

    - stressor: "D_sep (Semantic Drift)"
      description: "A measure of topological cohesion and cluster integrity."
      mechanics:
        cloud_effect: "If intra-class variance ($d_{intra}$) exceeds inter-class distance ($d_{inter}$), classes exist as a single multi-modal cloud."
        decision_manifold: "A high $D_{sep}$ implies a jagged and complex decision surface, causing high error in overlapping regions."

  stress_interaction_matrix:
    - interaction: "High $D_{imb}$ + High $D_{over}$"
      failure_mode: "The Camouflage Effect: Minority samples are statistically buried inside majority clusters."
      mitigation: "Focal Loss + SMOTE Oversampling"
    - interaction: "High $D_{dim}$ + High $D_{noise}$"
      failure_mode: "The Ghost Feature Trap: The model learns noise (slang/formatting) as a valid signal (Overfitting)."
      mitigation: "Dropout Layers + L1 Regularization"
    - interaction: "Low $D_{sep}$ + High $D_{dim}$"
      failure_mode: "Linear Collapse: Linear models fail to find any valid hyperplane due to data diffusion."
      mitigation: "Non-linear Kernels (RBF) or Deep MLPs"

  theoretical_significance:
    concept: "Pre-computation Complexity Bound"
    impact: "Estimates the VC-Dimension required for a model to 'shatter' the dataset, transforming the script into a diagnostic framework for unstructured text."
