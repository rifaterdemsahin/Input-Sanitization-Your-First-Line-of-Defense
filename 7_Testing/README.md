# ✅ 7_Testing: Validation & Quality Assurance

- **Premise:** A project is only complete when proven to work.
- **Content:** Testing scripts and documentation.
- **Conclusion:** Guarantees quality and confirms objectives are met.

## 🧪 Validation Strategy

### Test Metrics

#### 1. Clean Accuracy (Before Sanitization)
**Definition**: Model accuracy on unmodified test data.
```python
clean_acc_before = model.evaluate(X_test, y_test)
```
**Baseline**: Should match original model accuracy (~98% for MNIST)

#### 2. Clean Accuracy (After Sanitization)
**Definition**: Model accuracy on sanitized clean images.
```python
X_test_sanitized = sanitize_input(X_test)
clean_acc_after = model.evaluate(X_test_sanitized, y_test)
```
**Target**: >95% (acceptable quality loss <3%)

#### 3. Attack Success Rate (Before Defense)
**Definition**: Percentage of adversarial examples that fool the model.
```python
adv_predictions = model.predict(X_adv).argmax(axis=1)
asr_before = (adv_predictions != y_true).mean() * 100
```
**Baseline**: Typically 80-95% for strong attacks

#### 4. Attack Success Rate (After Defense)
**Definition**: Percentage of sanitized adversarial examples that fool the model.
```python
X_adv_sanitized = sanitize_input(X_adv)
adv_predictions_sanitized = model.predict(X_adv_sanitized).argmax(axis=1)
asr_after = (adv_predictions_sanitized != y_true).mean() * 100
```
**Target**: <40% (>50% reduction from baseline)

#### 5. Defense Effectiveness
**Definition**: Percentage reduction in attack success rate.
```python
defense_effectiveness = ((asr_before - asr_after) / asr_before) * 100
```
**Target**: >50% (halving attack success rate)

#### 6. Image Quality - PSNR
**Definition**: Peak Signal-to-Noise Ratio (measures pixel-level similarity).
```python
def calculate_psnr(original, sanitized):
    mse = np.mean((original - sanitized) ** 2)
    if mse == 0:
        return float('inf')
    max_pixel = 1.0  # assuming normalized images
    psnr = 20 * np.log10(max_pixel / np.sqrt(mse))
    return psnr
```
**Target**: >30 dB (good quality), >40 dB (excellent quality)

#### 7. Image Quality - SSIM
**Definition**: Structural Similarity Index (measures perceptual similarity).
```python
from skimage.metrics import structural_similarity as ssim
ssim_score = ssim(original, sanitized, multichannel=True)
```
**Target**: >0.90 (minimal perceptual difference)

### Comprehensive Evaluation Table

The notebook generates a detailed comparison:

| Defense Technique | Clean Acc | ASR (Before) | ASR (After) | Effectiveness | PSNR | SSIM |
|-------------------|-----------|--------------|-------------|---------------|------|------|
| None (Baseline) | 98.2% | 87.3% | 87.3% | 0% | ∞ | 1.00 |
| Feature Squeezing | 96.8% | 87.3% | 52.1% | 40.3% | 32.4 | 0.94 |
| JPEG Compression | 97.5% | 87.3% | 38.7% | 55.7% | 35.2 | 0.96 |
| Gaussian Blur | 97.1% | 87.3% | 45.6% | 47.8% | 33.8 | 0.95 |
| Combined Pipeline | 96.2% | 87.3% | 28.9% | 66.9% | 30.1 | 0.92 |

### Defense-Specific Tests

#### Feature Squeezing Tests
```python
# Test different bit depths
for bit_depth in [3, 4, 5, 6]:
    squeezed = reduce_bit_depth(X_adv, from_bits=8, to_bits=bit_depth)
    asr = evaluate_attack_success_rate(model, squeezed, y_true)
    quality = calculate_psnr(X_clean, squeezed)
    print(f"Bit depth {bit_depth}: ASR={asr:.1f}%, PSNR={quality:.1f} dB")
```

#### JPEG Compression Tests
```python
# Test different quality levels
for quality in [50, 65, 75, 85, 95]:
    compressed = jpeg_compression_defense(X_adv, quality=quality)
    asr = evaluate_attack_success_rate(model, compressed, y_true)
    psnr = calculate_psnr(X_clean, compressed)
    print(f"Quality {quality}: ASR={asr:.1f}%, PSNR={psnr:.1f} dB")
```

#### Gaussian Blur Tests
```python
# Test different kernel sizes
for kernel_size in [3, 5, 7, 9]:
    blurred = gaussian_blur_defense(X_adv, kernel_size=kernel_size)
    asr = evaluate_attack_success_rate(model, blurred, y_true)
    ssim_score = calculate_ssim(X_clean, blurred)
    print(f"Kernel {kernel_size}x{kernel_size}: ASR={asr:.1f}%, SSIM={ssim_score:.3f}")
```

### Adaptive Attack Robustness Test

Test if defense works against attacks that know about sanitization:
```python
# Generate adaptive attack (attack sanitized images directly)
def adaptive_attack(model, images, labels, sanitize_fn, epsilon):
    def sanitized_model(x):
        return model(sanitize_fn(x))
    
    return pgd_attack(sanitized_model, images, labels, epsilon)

X_adv_adaptive = adaptive_attack(model, X_test, y_test, sanitize_input, 0.3)
asr_adaptive = evaluate_attack_success_rate(model, sanitize_input(X_adv_adaptive), y_test)
```

### Validation Cells in Notebook
The notebook contains specific cells to:
1.  Evaluate each defense technique individually
2.  Test combined defense pipeline
3.  Measure clean accuracy degradation
4.  Calculate attack success rate reduction
5.  Compute image quality metrics (PSNR, SSIM)
6.  Visualize tradeoff curves (defense strength vs quality)
7.  Test adaptive attack robustness
8.  Generate comprehensive comparison tables
