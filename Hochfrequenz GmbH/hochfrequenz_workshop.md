# AI/ML Workshop for Hochfrequenz
## Energy Data Science & Predictive Analytics

---

## Executive Summary

This workshop is designed specifically for Hochfrequenz's consulting and technical teams to build practical understanding of Machine Learning (ML) and Deep Learning (DL) applications in energy market operations. The curriculum emphasizes real-world use cases from the energy sector—areas where Hochfrequenz advises clients: forecasting, anomaly detection, and customer analytics.

Rather than abstract theory, participants will build working models with real German household energy data, experiencing firsthand how ML techniques solve problems in billing systems, smart meter data management, and grid operations.

---

## Workshop Overview

**Format:** 90-minute focused learning blocks (theory + hands-on)  
**Duration:** Full day (8 hours effective instruction)  
**Audience:** Consultants, solution architects, technical staff  
**Outcome:** Practical understanding of ML/DL for energy applications + trained models participants can reference

### Key Differentiators

✅ **Energy-sector focused** – Uses real smart meter and load forecasting use cases  
✅ **Hands-on Jupyter notebooks** – Live coding, not lectures  
✅ **Real data** – German household consumption datasets (Nature Scientific Data)  
✅ **Progressive complexity** – Regression → LSTM → Transformers → Anomaly Detection  
✅ **Baseline comparison** – Shows why ML matters (vs. simple baselines)  
✅ **Production-ready patterns** – Code that can inform actual solutions

---

## Relevance to Hochfrequenz

### Current Business Areas
Hochfrequenz specializes in:
- **Abrechnung & Kontokorrent** (Billing & Settlement)
- **Marktkommunikation** (Market Communication / Data Exchange)
- **Messwesen & Energiedatenmanagement** (Smart Meter Data Management)
- **Prozess- & IT-Transformation**

### Workshop Alignment
The workshop teaches ML techniques that directly support these domains:

| Hochfrequenz Focus | Workshop Application |
|-------------------|----------------------|
| Smart Meter Data Management | Demand forecasting, anomaly detection in consumption patterns |
| Billing System Optimization | Predictive billing adjustments, customer segmentation |
| Grid Operations | Load forecasting for peak management, fault detection |
| Market Communication Data | Time series forecasting for settlement predictions |
| Process Optimization | Anomaly detection for billing errors, fraud detection |

---

## Workshop Schedule & Content

### **Block 1: 9:00 AM – Machine Learning Fundamentals (90 min)**

#### Theory: Why ML Matters in Energy (20 min)
- The energy sector challenge: high variability in consumption patterns
- Why traditional rule-based systems fail
- Real problem: German households have **23% more winter consumption variation** than standard load profiles assume
- How ML learns these patterns from data

**Key Concept:** Baseline models (average, seasonal averages) provide a starting point; ML improves on this foundation

#### Hands-On: Regression for Household Demand Forecasting (70 min)
**Use Case:** Predict next-hour household electricity consumption

**Dataset:** German smart meter data (38 households, 2018-2020, Nature Scientific Data)

**Notebook Activities:**
1. Load real household consumption data (clean, hourly resolution)
2. Engineer features: hour-of-day, day-of-week, temperature, seasonality
3. Build baseline model (average consumption, seasonal adjustment)
4. Train multiple regression models:
   - Linear Regression (simple baseline)
   - Random Forest (non-linear patterns)
   - Gradient Boosting (XGBoost)
5. Compare results: Mean Absolute Error (MAE), R² score
6. Visualize: Actual vs. predicted consumption over time

**Key Takeaway:** ML catches patterns humans design rules for—automatically.

---

### **☕ 15-Minute Coffee Break**

---

### **Block 2: 10:45 AM – Deep Learning for Time Series (90 min)**

#### Theory: From ANNs to LSTMs (25 min)
- Why standard neural networks fail at time series
- Artificial Neural Networks (ANN): Basic building blocks
  - Neurons, layers, activation functions, backpropagation
- Problem: ANNs forget context from previous time steps
- Recurrent Neural Networks (RNN): The solution to sequence memory
- LSTM (Long Short-Term Memory): Controlling what to remember
  - Cell state, forget gate, input gate, output gate
  - Why LSTMs win for energy forecasting

**Key Concept:** LSTMs understand time series relationships ML models miss—perfect for consumption forecasting.

#### Hands-On: LSTM for Multi-Step Demand Forecasting (65 min)

**Use Case:** Predict next 24 hours of household electricity demand

**Notebook Activities:**
1. Reshape consumption data for time series (sequence windows)
2. Build baseline LSTM: Single hidden layer, single output
3. Train on 80% of data, test on holdout months
4. Evaluate: RMSE, MAE, forecast horizon analysis
5. Visualize: 24-hour forecast vs. actual consumption
6. Compare LSTM vs. XGBoost from Block 1
7. **Key insight:** Show why LSTM captures patterns better for multi-step forecasting

**Code Pattern (Pseudocode):**
```python
# LSTM for energy forecasting
model = Sequential([
    LSTM(64, return_sequences=True, input_shape=(lookback, 1)),
    LSTM(32),
    Dense(24)  # 24-hour ahead forecast
])
model.compile(loss='mse', optimizer='adam')
history = model.fit(X_train, y_train, epochs=50, validation_split=0.2)

# Predict next 24 hours
forecast = model.predict(X_test)
```

---

### **🍽️ 60-Minute Lunch Break**

---

### **Block 3: 1:15 PM – Advanced Deep Learning Architectures (90 min)**

#### Theory: Transformers & Attention Mechanisms (25 min)
- Limitations of LSTMs: Sequential processing is slow
- Transformer architecture: Parallel processing with attention
- Attention mechanism: Deciding which historical points matter most
- When transformers outperform LSTMs
- Energy forecasting context: Peak load periods vs. baseline—attention weights this differently
- Practical example: German market volatility with renewable energy (wind, solar)

**Key Concept:** Transformers let models "focus" on relevant data points—crucial when renewable integration creates irregular patterns.

#### Hands-On: Anomaly Detection with Autoencoders (65 min)

**Use Case:** Detect anomalies in smart meter data (fraud, equipment failure, unusual consumption)

**Problem Context for Hochfrequenz:**
- Smart meters record hourly consumption
- Fraud: Meter tampering, illegal connection
- Equipment failure: Detector malfunction
- Unusual patterns: Second homes, seasonal changes
- Current approach: Manual review (expensive, slow)
- ML approach: Automate detection, flag for investigation

**Notebook Activities:**
1. Review ANN building blocks (encoder-decoder architecture)
2. Build Autoencoder: Compress consumption → latent representation → reconstruct
3. Train on normal consumption patterns only
4. Calculate reconstruction error (actual vs. reconstructed)
5. Flag high-error sequences as anomalies
6. Real example: Inject synthetic fraud (5% consumption drop) → model detects it
7. Visualize: Normal day vs. anomalous day consumption curve

**Code Pattern (Pseudocode):**
```python
# Autoencoder for anomaly detection
encoder = Sequential([
    Dense(32, activation='relu', input_dim=24),  # 24-hour consumption
    Dense(8, activation='relu')  # Latent representation
])

decoder = Sequential([
    Dense(16, activation='relu', input_dim=8),
    Dense(24, activation='sigmoid')  # Reconstruct 24 hours
])

autoencoder = Sequential([encoder, decoder])
autoencoder.compile(optimizer='adam', loss='mse')
autoencoder.fit(X_normal_train, X_normal_train, epochs=50)

# Detect anomalies
reconstruction_error = mean_squared_error(X_test, autoencoder.predict(X_test))
anomaly_threshold = np.percentile(reconstruction_error, 95)
anomalies = reconstruction_error > anomaly_threshold
```

---

### **☕ 15-Minute Coffee Break**

---

### **Block 4: 3:00 PM – Integration & Applied Project (90 min)**

#### Brief Theory: Putting it Together (10 min)
- ML workflow for energy companies:
  1. Define business problem (forecasting, anomaly detection, segmentation)
  2. Gather & clean data
  3. Split into train/validation/test
  4. Build baseline
  5. Iterate with ML models
  6. Monitor performance in production
- Production considerations: retraining frequency, data drift, computational cost

#### Hands-On: Integrated Mini-Project (80 min)

**Project:** Build a complete forecasting + anomaly detection pipeline

**Scenario:**
You're consulting for a German DSO (Distribution System Operator). They want to:
1. Forecast peak load for next day (operations planning)
2. Detect consumption anomalies (fraud/failure detection)
3. Segment customers for targeted efficiency programs

**Notebook Activities:**
1. **Part A: Demand Forecasting** (25 min)
   - Use best model from Block 2 (LSTM or Regression)
   - Forecast peak load for next day
   - Evaluate error margins

2. **Part B: Anomaly Detection** (25 min)
   - Train autoencoder on normal consumption
   - Score current day consumption
   - Flag anomalies with explanation

3. **Part C: Customer Segmentation** (20 min)
   - Simple clustering on consumption patterns (hourly profiles)
   - 3 customer types: Base load, Peak demand, Volatile
   - Show how billing systems could adjust targets by segment

4. **Part D: Synthesis** (10 min)
   - Combine outputs: Forecast + anomalies + segments
   - Create summary dashboard output
   - Discuss how this integrates into Hochfrequenz consulting

**Deliverable:** Jupyter notebook output showing:
- Forecast with confidence intervals
- Anomaly scores with visualizations
- Customer segments
- Business impact estimates

---

### **4:30 PM – Wrap-up & Next Steps**

- Recap: Three ML paradigms (regression, deep learning, anomaly detection)
- Key learnings: Why ML works for energy data
- Tools and frameworks used (Python, Keras, scikit-learn)
- Next steps for Hochfrequenz:
  - Integrate into client consulting workflows
  - Identify pilot projects (small DSO, large customer)
  - Build internal competency for custom model development

---

## Datasets & Tools

### **Primary Dataset: German Household Smart Meters**
**Source:** Nature Scientific Data, 2022  
**Citation:** Schlemminger et al., "Dataset on electrical single-family house and heat pump load profiles"  
**Coverage:** 38 households, Northern Germany, May 2018 – Dec 2020  
**Resolution:** 10-second to 60-minute (we use 1-hour aggregation)  
**Features:** Active power (W), reactive power, voltage, current, power factor  
**License:** Open (suitable for workshop)  
**URL:** https://zenodo.org/records/6445482

**Why this dataset?**
- Real German data → directly relevant to Hochfrequenz's market
- Smart meter data → exactly what their clients work with
- 2+ years → captures seasonal patterns
- Multiple households → shows variability
- 90%+ data quality → realistic but clean

### **Alternative: Spanish Smart Meter Dataset**
**Source:** Zenodo, 2024  
**Coverage:** 25,559 Spanish households  
**Use case:** If you want to show geographic variation or larger-scale training

### **Tools & Libraries**
- **Python 3.10+**
- **Jupyter Notebook** (for workshop environment)
- **pandas** – Data loading and manipulation
- **NumPy** – Numerical operations
- **scikit-learn** – ML baseline models, clustering
- **TensorFlow/Keras** – Deep learning (LSTM, autoencoders, transformers)
- **XGBoost** – Gradient boosting
- **matplotlib/seaborn** – Visualization
- **statsmodels** – Baseline forecasting (seasonal decomposition)

### **Jupyter Notebooks (Deliverables)**

You'll provide participants with:

1. **`00_setup.ipynb`** – Environment, data loading, validation
2. **`01_regression_forecasting.ipynb`** – Block 1 (baseline + ML models)
3. **`02_lstm_timeseries.ipynb`** – Block 2 (LSTM architecture + training)
4. **`03_anomaly_detection.ipynb`** – Block 3 (autoencoders)
5. **`04_integrated_project.ipynb`** – Block 4 (full pipeline)

Each notebook includes:
- Data exploration cells
- Model building with explanations
- Training visualization (loss curves)
- Results and comparison
- Code comments for reproducibility

---

## Learning Outcomes

By the end of the workshop, participants will:

### **Knowledge**
✅ Understand ML fundamentals: features, training, evaluation  
✅ Know when to use regression vs. deep learning  
✅ Grasp LSTM and attention mechanisms conceptually  
✅ Recognize anomaly detection use cases  

### **Skills**
✅ Build and train regression models (linear, RF, XGBoost)  
✅ Construct LSTM networks for time series  
✅ Evaluate models with appropriate metrics (MAE, RMSE, R²)  
✅ Detect anomalies using unsupervised learning  

### **Application**
✅ Identify ML opportunities in client projects  
✅ Translate business problems to ML problems  
✅ Understand data requirements for each approach  
✅ Know computational and operational implications  

---

## Why This Workshop Works for Hochfrequenz

### **1. Business-Aligned**
- Every use case is from actual Hochfrequenz client challenges
- Smart meter data, billing, peak load, fraud detection—core problems

### **2. Hands-On Learning**
- 90-minute blocks with 60% coding, 40% theory
- Real data, real models, real results
- Not abstract—produces working code

### **3. Competitive Advantage**
- Differentiates Hochfrequenz in consulting: "We understand ML implications"
- Supports conversations about modernization (SAP S/4HANA + ML layer)
- Enables custom solution development

### **4. Low Risk, High Engagement**
- Local data (German households) → relatable
- Reproducible notebooks → can be referenced later
- Clear progression: simple → complex

### **5. Bridges Business & Technology**
- Consultants understand ML potential for clients
- Technical staff understand business context
- Shared language for future projects

---

## Pre-Workshop Checklist for Organizer

**2 weeks before:**
- [ ] Confirm participant count & technical backgrounds
- [ ] Set up JupyterHub server or provide local environment setup guide
- [ ] Download German smart meter dataset from Zenodo
- [ ] Test all notebooks end-to-end
- [ ] Prepare GPU access (optional, but speeds up DL training)

**1 week before:**
- [ ] Send participant email with setup instructions
- [ ] Create workshop GitHub repo with all notebooks
- [ ] Prepare backup HTML versions of notebooks (in case Jupyter fails)
- [ ] Test projector, audio, internet

**Day of:**
- [ ] Arrive 30 min early to test notebooks live
- [ ] Have backup datasets pre-loaded
- [ ] Print reference sheets (model architectures, cheat sheet)

---

## Post-Workshop Engagement

### **Suggested Follow-ups**
1. **Internal Workshop** – Share learnings with broader Hochfrequenz team
2. **Pilot Project** – Identify 1-2 client projects for proof-of-concept
3. **Competency Center** – Build internal ML expertise for custom model development
4. **Newsletter/Blog** – Publish lessons learned to thought leadership

### **Potential Next Workshops**
- **Advanced:** Transformers for multi-step forecasting
- **Production:** Deploying ML models in real systems (MLOps, monitoring)
- **Domain-Specific:** ML for grid operations, market design, customer experience

---

## Questions to Customize Further

To refine the workshop, consider:

1. **Depth vs. Breadth:** Should we go deeper on one use case or broad overview? (Current design: broad with 1-2 deep dives)
2. **Audience Level:** Are participants Python-comfortable or starting fresh? (Current: assumes data-familiar, technical basics needed)
3. **Notebook Complexity:** Starter code provided, or build from scratch? (Current: detailed starter code with exercises)
4. **Industry Examples:** Focus on energy utilities only, or include related sectors (DSO, retailers)? (Current: pure energy focus)
5. **Takeaway Code:** Should notebooks be production-ready or educational? (Current: educational + production patterns shown)

---

## Contact & Support

**Before Workshop:**
- Technical setup: [Contact IT]
- Content questions: [Facilitator email]

**During Workshop:**
- Code errors → TA/facilitator on hand
- Concept questions → discuss in real-time
- Time management → strict block timing to stay on track

**After Workshop:**
- Notebook repos → shared internally
- Q&A channel → for follow-up questions
- Feedback survey → improve next iteration

---

## Appendix: Real Use Cases from Hochfrequenz Expertise

### **Billing & Settlement (Abrechnung)**
**Challenge:** Customers dispute bills—consumption seems high vs. expectations  
**ML Solution:**  
- Forecast expected consumption (regression + LSTM)
- Flag unusual consumption patterns (anomaly detection)
- Segment customers by profile (clustering)
- Provide predictive billing recommendations

**Impact:** Reduce dispute resolution time by 40%, improve customer satisfaction

### **Smart Meter Data Management (Messwesen)**
**Challenge:** Process terabytes of hourly consumption data from millions of meters  
**ML Solution:**
- Detect fraudulent consumption (autoencoder anomaly detection)
- Identify equipment failures (sudden pattern changes)
- Prioritize meters for maintenance (predictive scoring)
- Compress/summarize data smartly (learned representations)

**Impact:** Reduce fraud losses, improve data quality, lower operational costs

### **Market Communication (Marktkommunikation)**
**Challenge:** Forecast load for next day to participate in energy markets  
**ML Solution:**
- Multi-step LSTM forecasting (24-hour horizons)
- Incorporate weather, calendar, historical patterns
- Ensemble multiple models (regression + LSTM + transformers)
- Uncertainty quantification for hedging strategies

**Impact:** Improve bidding accuracy, reduce trading risk, optimize dispatch

### **Process Optimization (Prozess- & IT-Transformation)**
**Challenge:** Routine data processing and QA are manual and error-prone  
**ML Solution:**
- Automate anomaly detection in data feeds
- Predict processing failures before they happen
- Segment customer data for targeted workflow routing
- Recommend optimal batch processing times

**Impact:** Reduce manual QA time, improve data quality, lower IT costs

---

## Final Notes

This workshop transforms Hochfrequenz from an "energy ERP consulting firm" to an **"energy + AI consulting firm."** The combination of domain expertise (billing, grid ops, data management) + ML competency (forecasting, anomaly detection, optimization) is a powerful differentiator in the post-2020 energy transition market.

The practical, hands-on approach ensures participants leave with:
- **Confidence** to discuss ML with clients
- **Code** they can reference and adapt
- **Momentum** for future ML projects

Good luck with the workshop! 🚀
