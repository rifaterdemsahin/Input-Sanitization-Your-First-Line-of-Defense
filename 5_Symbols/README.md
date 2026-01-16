# 💻 5_Symbols: Implementation & Code

- **Premise:** Where theory becomes reality.
- **Content:** The core application source code (Jupyter Notebook).
- **Conclusion:** The heart of the project.

## 🔑 The Code
This folder contains the core implementation artifact:

### [input_sanitization_demo.ipynb](input_sanitization_demo.ipynb)
The Jupyter Notebook that demonstrates the full input sanitization defense:
1.  **Dataset Loading**: MNIST/CIFAR-10 image classification
2.  **Model Training**: Baseline classifier
3.  **Attack Generation**: FGSM and PGD adversarial examples
4.  **Defense 1**: Feature squeezing implementation
5.  **Defense 2**: JPEG compression implementation
6.  **Defense 3**: Gaussian filtering implementation
7.  **Combined Pipeline**: All three defenses together
8.  **Effectiveness Evaluation**: Attack success rate reduction
9.  **Quality Analysis**: PSNR/SSIM on clean images
10. **Visualization**: Before/after comparisons
11. **Adaptive Attacks**: Discussion of limitations
