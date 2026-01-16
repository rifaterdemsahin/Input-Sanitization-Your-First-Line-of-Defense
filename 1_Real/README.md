# 🎯 1_Real: Objectives & Key Results

- **Premise:** Every project must begin with a clear and measurable goal. This folder establishes the **"why"** behind the work.
- **Content:** High-level objectives and key results (OKRs).
- **Conclusion:** Aligns all work with a tangible purpose.

## 📌 Project Objective
To demonstrate how **Input Sanitization** serves as a first-layer defense against adversarial attacks by preprocessing inputs to remove perturbations.

### Core Concept: Defense in Depth
-   **No single defense is perfect**: Attackers can adapt to any single technique
-   **Layered defenses**: Force attackers to adapt repeatedly
-   **Input sanitization**: Your first layer of defense
-   **Goal**: Make adversarial perturbations ineffective without corrupting legitimate inputs

### Three Sanitization Techniques

#### 1. Feature Squeezing
-   Adversarial examples rely on fine-grained pixel values
-   Squeezing reduces bit depth (e.g., 8-bit → 4-bit)
-   Perturbations designed for 8-bit space become ineffective at 4-bit
-   **Tradeoff**: Some image quality lost for legitimate inputs

#### 2. JPEG Compression
-   JPEG compression discards high-frequency details
-   Many adversarial perturbations exist in high-frequency space
-   Compress to JPEG (lossy), then decompress back
-   Clean images survive with minimal quality loss
-   Adversarial perturbations degraded significantly

#### 3. Gaussian Filtering
-   Blur image with Gaussian filter (smoothing)
-   Adversarial perturbations are sharp and localized
-   Blurring averages out sharp changes
-   Clean objects remain recognizable
-   Adversarial effects diminish

### Goals
-   **Goal 1**: Implement all three sanitization techniques
-   **Goal 2**: Demonstrate defense effectiveness against FGSM/PGD attacks
-   **Goal 3**: Measure quality degradation on clean inputs
-   **Key Result**: Reduce attack success rate by >50% while maintaining >95% clean accuracy
