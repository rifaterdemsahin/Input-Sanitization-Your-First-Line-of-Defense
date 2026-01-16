# 📚 4_Formula: Guides & Best Practices

- **Premise:** Don't reinvent the wheel.
- **Content:** Essential guides, formulas, and code snippets.
- **Conclusion:** Solves challenges efficiently and ensures high quality.

## 🧪 The Formulas (Algorithms)

### Complete Sanitization Pipeline
```python
def sanitize_input(image):
    # Step 1: Feature squeezing (bit depth reduction)
    squeezed = reduce_bit_depth(image, from_bits=8, to_bits=4)
    
    # Step 2: JPEG compression
    jpeg_encoded = tf.image.encode_jpeg(squeezed, quality=95)
    decompressed = tf.image.decode_jpeg(jpeg_encoded)
    
    # Step 3: Gaussian filtering
    filtered = gaussian_blur(decompressed, kernel_size=5)
    
    return filtered
```

### Technique 1: Feature Squeezing
```python
def reduce_bit_depth(image, from_bits=8, to_bits=4):
    """
    Reduce color depth by quantization.
    
    Args:
        image: Input image with values in [0, 1]
        from_bits: Original bit depth (default: 8)
        to_bits: Target bit depth (default: 4)
    
    Returns:
        Squeezed image
    """
    # Calculate quantization levels
    max_value_from = 2 ** from_bits - 1
    max_value_to = 2 ** to_bits - 1
    
    # Quantize
    image_int = (image * max_value_from).astype(np.uint8)
    squeezed = (image_int // (2 ** (from_bits - to_bits))) * (2 ** (from_bits - to_bits))
    
    # Normalize back to [0, 1]
    return squeezed.astype(np.float32) / max_value_from
```

### Technique 2: JPEG Compression
```python
def jpeg_compression_defense(image, quality=75):
    """
    Apply JPEG compression to remove high-frequency perturbations.
    
    Args:
        image: Input image (numpy array or tensor)
        quality: JPEG quality (1-100, lower = more compression)
    
    Returns:
        Decompressed image
    """
    # Convert to uint8 if needed
    if image.max() <= 1.0:
        image_uint8 = (image * 255).astype(np.uint8)
    else:
        image_uint8 = image.astype(np.uint8)
    
    # Encode to JPEG
    encoded = cv2.imencode('.jpg', image_uint8, 
                          [cv2.IMWRITE_JPEG_QUALITY, quality])[1]
    
    # Decode back
    decoded = cv2.imdecode(encoded, cv2.IMREAD_COLOR)
    
    # Normalize back to [0, 1]
    return decoded.astype(np.float32) / 255.0
```

### Technique 3: Gaussian Filtering
```python
def gaussian_blur_defense(image, kernel_size=5, sigma=1.0):
    """
    Apply Gaussian blur to smooth out adversarial perturbations.
    
    Args:
        image: Input image
        kernel_size: Size of Gaussian kernel (must be odd)
        sigma: Standard deviation of Gaussian
    
    Returns:
        Blurred image
    """
    return cv2.GaussianBlur(image, (kernel_size, kernel_size), sigma)
```

### Defense Effectiveness Metrics

#### Attack Success Rate (Before Defense)
```python
asr_before = (model.predict(adv_images).argmax(axis=1) != true_labels).mean()
```

#### Attack Success Rate (After Defense)
```python
sanitized_images = sanitize_input(adv_images)
asr_after = (model.predict(sanitized_images).argmax(axis=1) != true_labels).mean()
```

#### Defense Effectiveness
```python
defense_effectiveness = (asr_before - asr_after) / asr_before * 100
```

#### Image Quality Metrics
```python
# Peak Signal-to-Noise Ratio (higher is better)
psnr = calculate_psnr(original_image, sanitized_image)

# Structural Similarity Index (higher is better)
ssim = calculate_ssim(original_image, sanitized_image)
```

## 📐 Layered Defense Strategy

### Single-Layer Defense
```python
# Just one technique
defended = jpeg_compression_defense(adv_image, quality=75)
```

### Multi-Layer Defense (Recommended)
```python
# Stack multiple defenses
defended = adv_image
defended = reduce_bit_depth(defended, from_bits=8, to_bits=5)
defended = jpeg_compression_defense(defended, quality=85)
defended = gaussian_blur_defense(defended, kernel_size=3, sigma=0.5)
```

## ⚙️ Hyperparameter Tuning

### Feature Squeezing
-   **Bit depth**: 4-6 bits (4 = stronger defense, more quality loss)

### JPEG Compression
-   **Quality**: 75-95 (lower = stronger defense, more quality loss)

### Gaussian Filtering
-   **Kernel size**: 3-7 (larger = stronger defense, more blur)
-   **Sigma**: 0.5-2.0 (larger = more smoothing)
