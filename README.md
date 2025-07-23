# LaViT for Road Extraction: A Conceptual Implementation

This project is an educational exercise to understand and implement the core concepts of the paper "You Only Need Less Attention at Each Stage in Vision Transformers" (LaViT) for a semantic segmentation task.

We built a simplified, conceptual version of the model to learn the fundamental workflow of building, debugging, and improving a deep learning model.

## The Journey: Problems Faced & Solutions

This project was a step-by-step process of encountering common machine learning challenges and implementing the correct solutions.

### 1. Initial Problem: Unstable Training & Exploding Loss
- **Issue:** Our first model's training was unstable. The loss value suddenly became extremely large and negative.
- **Diagnosis:** The custom `diagonality_preserving_loss`, intended as a regularizer, was overpowering the main segmentation loss.
- **Fix:** We stabilized the training by temporarily setting its weight to zero (`lambda_dp = 0`).

### 2. Second Problem: Inaccurate & Disconnected Predictions
- **Issue:** The stable model produced blurry, disconnected splotches and failed to learn the continuous shape of the "road."
- **Diagnosis:** The simple Encoder-Decoder architecture was losing too much spatial information.
- **Fix:** We implemented **skip connections** between the encoder and decoder to feed high-resolution details to the upsampling path.

### 3. Third Problem: Overfitting
- **Issue:** When training for many epochs, the model's performance on unseen data (Validation Loss) got worse over time.
- **Diagnosis:** We identified this classic overfitting pattern by comparing Training Loss vs. Validation Loss.
- **Fix:** The solution is **early stopping**—using the version of the model from the epoch where the validation loss was at its minimum.
