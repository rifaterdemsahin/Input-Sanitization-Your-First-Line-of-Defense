# 🛡️ Input Sanitization: First-Layer Defense Against Adversarial Attacks

## 🎯 Overview
This project demonstrates **Input Sanitization** techniques that defend against adversarial attacks by preprocessing inputs before they reach the model.
The core asset is a Jupyter Notebook that implements three sanitization methods: Feature Squeezing, JPEG Compression, and Gaussian Filtering.

## 📂 Project Structure
The project follows a 7-stage holistic development lifecycle:

-   **[1_Real](1_Real/README.md)**: Objectives - Defense in depth philosophy (layered security).
-   **[2_Environment](2_Environment/README.md)**: Tools - TensorFlow/PyTorch, image processing libraries.
-   **[3_UI](3_UI/README.md)**: Interface - The notebook showing before/after sanitization.
-   **[4_Formula](4_Formula/README.md)**: Logic - Feature squeezing, JPEG compression, Gaussian filtering.
-   **[5_Symbols](5_Symbols/README.md)**: **Code - The `input_sanitization_demo.ipynb` lives here.**
-   **[6_Semblance](6_Semblance/README.md)**: Errors - Adaptive attacks and quality degradation issues.
-   **[7_Testing](7_Testing/README.md)**: Validation - Measuring defense effectiveness vs quality loss.

## 🚀 Getting Started
1.  Navigate to `5_Symbols/input_sanitization_demo.ipynb`.
2.  Open the file in a Jupyter environment (VS Code or Google Colab).
3.  Run the cells to see sanitization defenses in action.

## 💡 Key Insight
**Defense in Depth**: No single defense is perfect. Input sanitization is your first layer—it removes adversarial perturbations before they reach the model, forcing attackers to adapt their strategies repeatedly.