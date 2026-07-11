# Jupyter Notebook Templates for Blocks 2-4

This document provides structured templates for creating the remaining three notebooks. Each follows the same pattern as Block 1 for consistency.

---

## Block 2: LSTM for Multi-Step Time Series Forecasting

### Notebook Structure

```python
# BLOCK 2: LSTM for Time Series
# Duration: 90 minutes
# Theory: 25 min | Coding: 65 min

# === SECTION 1: Theory ===
# - From ANNs to RNNs to LSTMs (diagrams + markdown)
# - Why sequential models work for time series
# - LSTM cell anatomy (gates, memory, outputs)
# - Architecture diagram

# === SECTION 2: Recap from Block 1 ===
# - Load same data as Block 1
# - Feature engineering review
# - Train/test split

# === SECTION 3: LSTM Architecture ===
# - Build encoder-decoder LSTM
# - Visualize model architecture
# - Show parameter count

# === SECTION 4: Training ===
# - Prepare sequences (lookback window approach)
# - Train on training set
# - Plot loss curves

# === SECTION 5: Forecasting ===
# - Single-step forecasting (1h ahead)
# - Multi-step forecasting (24h ahead)
# - Rolling window approach

# === SECTION 6: Evaluation ===
# - Compare LSTM vs. XGBoost from Block 1
# - Error analysis (short-term vs. long-term)
# - Visualize forecasts

# === SECTION 7: Key Insights ===
# - When LSTM beats traditional ML
# - Limitations & overfitting risk
# - Production considerations
```

### Key Code Sections

```python
# ============= BUILDING SEQUENCES =============
def create_sequences(data, lookback=24, forecast_horizon=1):
    """Convert time series to supervised learning format."""
    X, y = [], []
    for i in range(len(data) - lookback - forecast_horizon):
        X.append(data[i:i+lookback])
        y.append(data[i+lookback:i+lookback+forecast_horizon])
    return np.array(X), np.array(y)

# Create sequences with 24-hour lookback, 1-hour forecast
X_seq, y_seq = create_sequences(data_scaled, lookback=24, forecast_horizon=1)
X_train_seq = X_seq[:split_idx]
y_train_seq = y_seq[:split_idx]


# ============= LSTM MODEL =============
from tensorflow import keras

model = keras.Sequential([
    keras.layers.LSTM(64, return_sequences=True, input_shape=(24, num_features)),
    keras.layers.Dropout(0.2),
    keras.layers.LSTM(32),
    keras.layers.Dropout(0.2),
    keras.layers.Dense(16, activation='relu'),
    keras.layers.Dense(1)  # Next hour consumption
])

model.compile(optimizer='adam', loss='mse', metrics=['mae'])
print(model.summary())


# ============= TRAINING =============
history = model.fit(
    X_train_seq, y_train_seq,
    epochs=50,
    batch_size=32,
    validation_split=0.2,
    verbose=1,
    callbacks=[
        keras.callbacks.EarlyStopping(monitor='val_loss', patience=5)
    ]
)

# Plot training history
fig, ax = plt.subplots(1, 2, figsize=(14, 4))
ax[0].plot(history.history['loss'], label='Training Loss')
ax[0].plot(history.history['val_loss'], label='Validation Loss')
ax[0].set_xlabel('Epoch')
ax[0].set_ylabel('Loss (MSE)')
ax[0].set_title('LSTM Training History')
ax[0].legend()

ax[1].plot(history.history['mae'], label='Training MAE')
ax[1].plot(history.history['val_mae'], label='Validation MAE')
ax[1].set_xlabel('Epoch')
ax[1].set_ylabel('MAE (W)')
ax[1].set_title('MAE Over Time')
ax[1].legend()
plt.tight_layout()


# ============= PREDICTION & EVALUATION =============
lstm_pred_test = model.predict(X_test_seq).flatten()

# Inverse transform back to original scale
lstm_pred_test_rescaled = scaler.inverse_transform(
    np.hstack([lstm_pred_test.reshape(-1, 1), X_test_seq[:, -1, 1:]])
)[:, 0]

# Evaluate
lstm_mae = mean_absolute_error(y_test_rescaled, lstm_pred_test_rescaled)
lstm_rmse = np.sqrt(mean_squared_error(y_test_rescaled, lstm_pred_test_rescaled))
lstm_r2 = r2_score(y_test_rescaled, lstm_pred_test_rescaled)

print(f"LSTM Performance:")
print(f"  MAE:  {lstm_mae:.2f} W")
print(f"  RMSE: {lstm_rmse:.2f} W")
print(f"  R²:   {lstm_r2:.3f}")
```

---

## Block 3: Anomaly Detection with Autoencoders

### Notebook Structure

```python
# BLOCK 3: ANOMALY DETECTION
# Duration: 90 minutes
# Theory: 25 min | Coding: 65 min

# === SECTION 1: Theory ===
# - ANN recap (building blocks for autoencoders)
# - Encoder-decoder architecture
# - Reconstruction error concept
# - Anomaly detection workflow

# === SECTION 2: Data Preparation ===
# - Load consumption data from Block 1
# - Define "normal" consumption (training data)
# - Synthetic anomalies (for validation)

# === SECTION 3: Autoencoder Architecture ===
# - Build encoder (input → latent representation)
# - Build decoder (latent → reconstruct input)
# - Show compression ratio

# === SECTION 4: Training on Normal Data ===
# - Train ONLY on normal consumption patterns
# - Monitor reconstruction loss
# - Visualize encoded representations

# === SECTION 5: Anomaly Detection ===
# - Calculate reconstruction error on test data
# - Set anomaly threshold
# - Detect and visualize anomalies

# === SECTION 6: Case Study ===
# - Inject synthetic fraud/failure
# - Verify detection
# - Calculate precision/recall

# === SECTION 7: Real-World Applications ===
# - Fraud detection use cases
# - Equipment failure indicators
# - Unusual consumption patterns
```

### Key Code Sections

```python
# ============= AUTOENCODER MODEL =============
from tensorflow import keras

# Input dimension: 24 hours of consumption
input_dim = 24

# Encoder
encoder = keras.Sequential([
    keras.layers.Dense(16, activation='relu', input_dim=input_dim),
    keras.layers.Dense(8, activation='relu'),
    keras.layers.Dense(4, activation='relu'),  # Latent representation
], name='encoder')

# Decoder
decoder = keras.Sequential([
    keras.layers.Dense(8, activation='relu', input_dim=4),
    keras.layers.Dense(16, activation='relu'),
    keras.layers.Dense(input_dim, activation='sigmoid'),  # Reconstruct 24 hours
], name='decoder')

# Full autoencoder
autoencoder = keras.Sequential([encoder, decoder], name='autoencoder')
autoencoder.compile(optimizer='adam', loss='mse')

print(f"Encoder compression: {input_dim} → 4 → {input_dim}")
print(f"Compression ratio: {4/input_dim:.1%}")


# ============= PREPARE DATA =============
# Create rolling windows of 24-hour consumption
def create_windows(data, window_size=24):
    windows = []
    for i in range(len(data) - window_size):
        windows.append(data[i:i+window_size])
    return np.array(windows)

X_windows = create_windows(data['consumption'].values, window_size=24)

# Normalize
scaler_ae = StandardScaler()
X_windows_scaled = scaler_ae.fit_transform(X_windows)

# Split: first 80% = normal (training), last 20% = test (may contain anomalies)
split_idx = int(len(X_windows_scaled) * 0.8)
X_train_normal = X_windows_scaled[:split_idx]
X_test_all = X_windows_scaled[split_idx:]


# ============= TRAINING =============
history_ae = autoencoder.fit(
    X_train_normal, X_train_normal,  # Unsupervised: input = target
    epochs=50,
    batch_size=32,
    validation_split=0.2,
    verbose=1,
    callbacks=[
        keras.callbacks.EarlyStopping(monitor='val_loss', patience=5)
    ]
)

# Plot reconstruction error
plt.figure(figsize=(12, 4))
plt.plot(history_ae.history['loss'], label='Training Loss')
plt.plot(history_ae.history['val_loss'], label='Validation Loss')
plt.xlabel('Epoch')
plt.ylabel('Reconstruction MSE')
plt.title('Autoencoder Training: Reconstruction Error')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()


# ============= ANOMALY DETECTION =============
# Calculate reconstruction error for all test windows
X_test_reconstructed = autoencoder.predict(X_test_all, verbose=0)
reconstruction_error = np.mean(np.power(X_test_all - X_test_reconstructed, 2), axis=1)

# Set threshold (e.g., 95th percentile)
threshold = np.percentile(reconstruction_error, 95)

# Detect anomalies
anomalies = reconstruction_error > threshold
anomaly_indices = np.where(anomalies)[0]

print(f"Reconstruction Error Statistics:")
print(f"  Mean: {reconstruction_error.mean():.4f}")
print(f"  Std:  {reconstruction_error.std():.4f}")
print(f"  95th percentile (threshold): {threshold:.4f}")
print(f"\nAnomalies detected: {anomalies.sum()} / {len(reconstruction_error)} ({anomalies.sum()/len(reconstruction_error)*100:.1f}%)")


# ============= VISUALIZATION =============
# Plot reconstruction error over time
fig, axes = plt.subplots(2, 1, figsize=(14, 8))

# Top: Reconstruction error time series
axes[0].plot(reconstruction_error, linewidth=1, alpha=0.7)
axes[0].axhline(y=threshold, color='r', linestyle='--', linewidth=2, label=f'Threshold: {threshold:.4f}')
axes[0].scatter(anomaly_indices, reconstruction_error[anomaly_indices], color='red', s=50, label='Anomalies')
axes[0].set_ylabel('Reconstruction Error (MSE)')
axes[0].set_title('Anomaly Detection: Reconstruction Error Over Time')
axes[0].legend()
axes[0].grid(True, alpha=0.3)

# Bottom: Actual vs Reconstructed for one normal and one anomalous window
normal_idx = np.argmin(reconstruction_error)  # Best reconstruction
anomaly_idx = anomaly_indices[0]  # First anomaly

axes[1].plot(X_test_all[normal_idx], 'o-', label='Normal (actual)', linewidth=2, markersize=4)
axes[1].plot(X_test_reconstructed[normal_idx], 's--', label='Normal (reconstructed)', linewidth=2, markersize=4)
axes[1].plot(X_test_all[anomaly_idx] + 0.5, 'o-', label='Anomaly (actual)', linewidth=2, markersize=4)
axes[1].plot(X_test_reconstructed[anomaly_idx] + 0.5, 's--', label='Anomaly (reconstructed)', linewidth=2, markersize=4)
axes[1].set_ylabel('Consumption (normalized)')
axes[1].set_xlabel('Hour')
axes[1].set_title('Example: Normal vs Anomalous Pattern Reconstruction')
axes[1].legend()
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()


# ============= SYNTHETIC FRAUD TEST =============
# Inject consumption reduction (meter tampering)
X_test_with_fraud = X_test_all.copy()
fraud_indices = np.random.choice(len(X_test_all), size=int(0.05 * len(X_test_all)), replace=False)
X_test_with_fraud[fraud_indices] *= 0.7  # 30% consumption reduction

# Detect in fraud dataset
X_fraud_reconstructed = autoencoder.predict(X_test_with_fraud, verbose=0)
reconstruction_error_fraud = np.mean(np.power(X_test_with_fraud - X_fraud_reconstructed, 2), axis=1)

fraud_detected = reconstruction_error_fraud > threshold

print(f"\n=== FRAUD DETECTION TEST ===")
print(f"Synthetic fraud injected: {len(fraud_indices)} windows (5%)")
print(f"Fraud patterns detected: {fraud_detected[fraud_indices].sum()} / {len(fraud_indices)}")
print(f"Detection rate (recall): {fraud_detected[fraud_indices].sum() / len(fraud_indices) * 100:.1f}%")
```

---

## Block 4: Integrated Pipeline

### Notebook Structure

```python
# BLOCK 4: INTEGRATED PROJECT - END-TO-END PIPELINE
# Duration: 90 minutes
# Intro: 10 min | Guided Coding: 60 min | Results Sharing: 20 min

# === SECTION 1: BUSINESS PROBLEM ===
# You're consulting for a German DSO.
# Three challenges:
#   1. Forecast peak load for next day
#   2. Detect meter anomalies (fraud/failure)
#   3. Segment customers for targeted programs

# === SECTION 2: DATA SETUP ===
# - Load cleaned data from Blocks 1-3
# - Prepare for multi-task pipeline
# - Define evaluation criteria

# === SECTION 3: FORECASTING PIPELINE ===
# - Use best model from Block 2 (likely XGBoost or LSTM)
# - Forecast next-day peak load
# - Provide confidence intervals

# === SECTION 4: ANOMALY DETECTION PIPELINE ===
# - Use autoencoder from Block 3
# - Score current meter data
# - Flag high-risk meters

# === SECTION 5: CUSTOMER SEGMENTATION ===
# - Cluster customers by consumption pattern
# - Label segments: "Base Load", "Peak Sensitive", "Volatile"
# - Show segment characteristics

# === SECTION 6: INTEGRATION ===
# - Combine all outputs
# - Create summary dashboard
# - Generate actionable recommendations

# === SECTION 7: BUSINESS IMPACT ===
# - Quantify cost savings
# - Discuss implementation challenges
# - Define next steps for client
```

### Key Code Sections

```python
# ============= INTEGRATED PIPELINE CLASS =============
class EnergyForecastingPipeline:
    """End-to-end energy forecasting and monitoring pipeline."""
    
    def __init__(self, forecast_model, anomaly_detector, scaler):
        self.forecast_model = forecast_model  # XGBoost or LSTM
        self.anomaly_detector = anomaly_detector  # Autoencoder
        self.scaler = scaler
        
    def forecast_next_day(self, recent_data):
        """Predict peak load for next 24 hours."""
        # Feature engineering
        X_forecast = self._prepare_features(recent_data)
        
        # Predict
        predictions = self.forecast_model.predict(X_forecast)
        
        return {
            'peak_load_mw': predictions.max(),
            'avg_load_mw': predictions.mean(),
            'hourly_forecast': predictions,
            'confidence': 'high' if predictions.std() < np.percentile(predictions, 75) else 'low'
        }
    
    def detect_anomalies(self, meter_data):
        """Score meter data for anomalies."""
        X_windows = self._create_windows(meter_data)
        reconstruction_error = self._calculate_error(X_windows)
        threshold = np.percentile(reconstruction_error, 95)
        
        anomaly_score = reconstruction_error / threshold  # Normalized score
        
        return {
            'anomaly_scores': anomaly_score,
            'threshold': threshold,
            'high_risk_meters': np.where(anomaly_score > 1.5)[0],
            'flagged_count': (anomaly_score > 1.5).sum()
        }
    
    def segment_customers(self, consumption_data):
        """Cluster customers by consumption patterns."""
        from sklearn.cluster import KMeans
        
        # Hourly pattern features for each customer
        features = self._extract_customer_patterns(consumption_data)
        
        # Cluster
        kmeans = KMeans(n_clusters=3, random_state=42)
        segments = kmeans.fit_predict(features)
        
        segment_names = ['Base Load', 'Peak Sensitive', 'Volatile']
        
        return {
            'segments': segments,
            'segment_names': segment_names,
            'characteristics': self._describe_segments(features, segments),
            'customer_to_segment': dict(enumerate(segments))
        }
    
    def _prepare_features(self, data):
        """Feature engineering for forecasting."""
        features = []
        features.append(data['hour'])
        features.append(data['day_of_week'])
        features.append(data['temperature'])
        features.append(data['consumption_lag_24'])
        return np.array(features).reshape(1, -1)
    
    def _create_windows(self, data, window_size=24):
        windows = []
        for i in range(len(data) - window_size):
            windows.append(data[i:i+window_size])
        return np.array(windows)
    
    def _calculate_error(self, X):
        X_reconstructed = self.anomaly_detector.predict(X, verbose=0)
        return np.mean(np.power(X - X_reconstructed, 2), axis=1)
    
    def _extract_customer_patterns(self, data):
        """Extract consumption pattern features per customer."""
        patterns = []
        for customer_id in data['customer_id'].unique():
            cust_data = data[data['customer_id'] == customer_id]
            # Get average hourly pattern
            hourly_avg = cust_data.groupby('hour')['consumption'].mean()
            patterns.append(hourly_avg.values)
        return np.array(patterns)
    
    def _describe_segments(self, features, segments):
        """Generate segment descriptions."""
        descriptions = {}
        for seg_id in range(3):
            seg_features = features[segments == seg_id]
            descriptions[f'Segment {seg_id}'] = {
                'size': len(seg_features),
                'avg_consumption': seg_features.mean(axis=0).mean(),
                'std_consumption': seg_features.std(axis=0).mean(),
                'peak_hour': np.argmax(seg_features.mean(axis=0))
            }
        return descriptions


# ============= MAIN ANALYSIS =============
# Initialize pipeline
pipeline = EnergyForecastingPipeline(
    forecast_model=xgb_model,  # From Block 1
    anomaly_detector=autoencoder,  # From Block 3
    scaler=scaler
)

# Run analysis on test data
print("=" * 60)
print("INTEGRATED PIPELINE RESULTS")
print("=" * 60)

# 1. Forecast
forecast_results = pipeline.forecast_next_day(data_test.iloc[-24:])
print("\n📊 FORECASTING:")
print(f"  Predicted peak load: {forecast_results['peak_load_mw']:.1f} MW")
print(f"  Average load: {forecast_results['avg_load_mw']:.1f} MW")
print(f"  Confidence: {forecast_results['confidence']}")

# 2. Anomaly detection
anomaly_results = pipeline.detect_anomalies(data_test['consumption'].values)
print("\n⚠️  ANOMALY DETECTION:")
print(f"  High-risk meters: {anomaly_results['flagged_count']} detected")
print(f"  Risk level: {'HIGH' if anomaly_results['flagged_count'] > 10 else 'NORMAL'}")

# 3. Segmentation
segment_results = pipeline.segment_customers(data_test)
print("\n👥 CUSTOMER SEGMENTATION:")
for seg_name, chars in segment_results['characteristics'].items():
    print(f"  {seg_name}:")
    print(f"    Size: {chars['size']} customers")
    print(f"    Avg: {chars['avg_consumption']:.0f} W, Std: {chars['std_consumption']:.0f} W")


# ============= VISUALIZATION DASHBOARD =============
fig = plt.figure(figsize=(16, 10))
gs = fig.add_gridspec(3, 2, hspace=0.3, wspace=0.3)

# 1. Forecast
ax1 = fig.add_subplot(gs[0, :])
hours = range(24)
ax1.plot(hours, forecast_results['hourly_forecast'], 'o-', linewidth=2, markersize=6, label='Forecast')
ax1.fill_between(hours, forecast_results['hourly_forecast']*0.9, forecast_results['hourly_forecast']*1.1, alpha=0.3)
ax1.set_ylabel('Load (MW)')
ax1.set_title('Next Day Forecast with Confidence Band')
ax1.grid(True, alpha=0.3)
ax1.legend()

# 2. Anomaly scores
ax2 = fig.add_subplot(gs[1, 0])
ax2.scatter(range(len(anomaly_results['anomaly_scores'])), 
           anomaly_results['anomaly_scores'], alpha=0.6, s=30)
ax2.axhline(y=1.0, color='r', linestyle='--', label='Alert threshold')
ax2.set_xlabel('Meter ID')
ax2.set_ylabel('Anomaly Score')
ax2.set_title('Meter Anomaly Scores')
ax2.legend()
ax2.grid(True, alpha=0.3)

# 3. Segment distribution
ax3 = fig.add_subplot(gs[1, 1])
segment_counts = np.bincount(segment_results['segments'])
ax3.bar(range(3), segment_counts, color=['blue', 'green', 'orange'])
ax3.set_xticks(range(3))
ax3.set_xticklabels(['Base Load', 'Peak Sensitive', 'Volatile'])
ax3.set_ylabel('# of Customers')
ax3.set_title('Customer Segment Distribution')
ax3.grid(True, alpha=0.3, axis='y')

# 4. Business impact summary
ax4 = fig.add_subplot(gs[2, :])
ax4.axis('off')

impact_text = f"""
BUSINESS IMPACT SUMMARY

✅ Forecasting: Better peak prediction → {5}% cost savings in demand management
⚠️  Anomaly Detection: {anomaly_results['flagged_count']} meters flagged → Potential fraud/failures
👥 Segmentation: {3} customer types identified → Targeted efficiency programs

Implementation Timeline:
  Week 1: Deploy forecasting model to production
  Week 2-3: Test anomaly detection on pilot meters
  Month 2: Launch customer segmentation programs
  Month 3+: Expand to full fleet, measure impact

Expected ROI:
  One-time: Implementation costs (~$50K)
  Annual: Demand management savings ($200K+)
  Payback: 3-4 months
"""

ax4.text(0.05, 0.95, impact_text, transform=ax4.transAxes, fontsize=11,
        verticalalignment='top', fontfamily='monospace',
        bbox=dict(boxstyle='round', facecolor='wheat', alpha=0.8))

plt.suptitle('Integrated Energy Management Pipeline Dashboard', fontsize=14, fontweight='bold')
plt.show()
```

---

## Template Usage

### To Create Block 2:

1. Copy the "Block 2: LSTM" structure above
2. Expand each section with markdown explanations
3. Add code cells from "Key Code Sections"
4. Run & test end-to-end
5. Add visualizations

### To Create Block 3:

1. Copy the "Block 3: Anomaly Detection" structure
2. Implement each section
3. Include the PipelineClass code
4. Test fraud detection case study
5. Verify detection rates

### To Create Block 4:

1. Copy "Block 4: Integrated" structure
2. Implement EnergyForecastingPipeline class
3. Run all three pipelines (forecast, anomaly, segment)
4. Generate dashboard visualization
5. Create business impact summary

---

## Quick Tips

- **Consistency:** Match notebook structure, styling, and tone with Block 1
- **Testing:** Run each notebook standalone (don't assume Block 1 variables exist)
- **Outputs:** Include all visualizations, make them publication-ready
- **Interactivity:** Add markdown cells explaining "why" after each code section
- **Error handling:** Add try/catch for common issues (dimension mismatches, etc.)

---

These templates should take 3-4 hours total to fill in with complete, production-ready notebooks.

Good luck! 🚀
