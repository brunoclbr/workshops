# Hochfrequenz AI Workshop - Implementation Guide

## Quick Start for Organizers

This guide helps you set up and deliver the full-day AI/ML workshop for Hochfrequenz.

---

## 📋 Pre-Workshop (2-4 Weeks Before)

### 1. Environment Setup

**Option A: Cloud Jupyter (Recommended)**
```bash
# Option A1: Google Colab (no setup needed)
# Share notebooks as .ipynb files
# Participants open in Google Colab

# Option A2: AWS SageMaker or Azure Notebooks
# Create shared notebook instances for participants
```

**Option B: Local Installation**
```bash
# Create conda environment
conda create -n hochfrequenz-workshop python=3.10
conda activate hochfrequenz-workshop

# Install dependencies
pip install -r requirements.txt
```

**requirements.txt**
```
pandas==2.0.0
numpy==1.24.0
matplotlib==3.7.0
seaborn==0.12.0
scikit-learn==1.2.0
xgboost==2.0.0
tensorflow==2.13.0
keras==2.13.0
jupyter==1.0.0
```

### 2. Download Real Data (Optional - Adds Authenticity)

```python
# Python script to download German smart meter dataset
import zenodo_client

# Schlemminger et al., Nature Scientific Data 2022
zenodo_id = "6445482"
zenodo_client.download(zenodo_id, output_dir="./data/")
```

**Alternative:** Use the synthetic data generator included in Block 1 notebook (sufficient for workshop)

### 3. Create Participant Materials

```
workshop/
├── notebooks/
│   ├── 00_setup.ipynb
│   ├── 01_regression_forecasting.ipynb       # Block 1
│   ├── 02_lstm_timeseries.ipynb              # Block 2
│   ├── 03_anomaly_detection.ipynb            # Block 3
│   └── 04_integrated_project.ipynb           # Block 4
├── data/
│   └── german_household_consumption.csv      # Real or synthetic
├── references/
│   ├── ml_cheat_sheet.pdf
│   ├── deep_learning_architectures.pdf
│   └── energy_use_cases.pdf
└── README.md
```

### 4. Pre-Workshop Communication

**Email Template (1 week before):**

```
Subject: Hochfrequenz AI Workshop - Setup & Logistics

Dear Workshop Participants,

We're excited for next [DAY] at [TIME]!

📍 Location: [ROOM] at Hochfrequenz HQ
🕐 Time: 9:00 AM - 4:30 PM (with breaks)

PREPARATION:
1. Ensure your laptop has Python 3.10+ installed
   - Windows: https://www.python.org/downloads/
   - Mac/Linux: Use Homebrew or system package manager

2. Clone workshop repository:
   $ git clone https://github.com/hochfrequenz/ai-workshop.git

3. Create environment:
   $ cd ai-workshop
   $ conda create -n workshop python=3.10
   $ conda activate workshop
   $ pip install -r requirements.txt

4. Test setup:
   $ jupyter notebook 00_setup.ipynb
   - You should see a working Jupyter environment

QUESTIONS? Email [contact]

See you soon! 🚀
```

---

## 🎯 Day-Of Logistics

### Morning Setup (30 min before)

**Checklist:**
- [ ] Test internet connection & WiFi
- [ ] Test projector + laptop HDMI
- [ ] Open all 4 notebooks locally (backup in case network issues)
- [ ] Download datasets locally (not streaming from Zenodo)
- [ ] Have printed reference sheets at each seat
- [ ] Backup slides (PDF) of key architectures
- [ ] Coffee & water ready

### Timing & Transitions

```
8:45 AM   - Participants arrive, settle in
9:00 AM   - Welcome + Agenda (5 min)
           
BLOCK 1: 9:05 AM - 10:30 AM (Regression)
  - Theory: 20 min
  - Live coding: 60 min
  - Wrap-up: 5 min

10:30 AM  - ☕ COFFEE BREAK (15 min)

BLOCK 2: 10:45 AM - 12:15 PM (LSTM)
  - Theory: 25 min
  - Live coding: 55 min
  - Wrap-up: 5 min

12:15 PM  - 🍽️ LUNCH (60 min)

BLOCK 3: 1:15 PM - 2:45 PM (Anomaly Detection)
  - Theory: 25 min
  - Live coding: 55 min
  - Wrap-up: 5 min

2:45 PM   - ☕ COFFEE BREAK (15 min)

BLOCK 4: 3:00 PM - 4:15 PM (Integrated Project)
  - Intro: 10 min
  - Guided coding: 60 min
  - Results sharing: 5 min

4:15 PM   - Final Q&A + Feedback (15 min)
4:30 PM   - END
```

---

## 🎓 Facilitation Tips

### During Live Coding

**Do:**
- ✅ Narrate what you're typing
- ✅ Pause after each cell to let people catch up
- ✅ Explain errors when they happen (they will!)
- ✅ Ask participants to follow along on their laptops
- ✅ Show output immediately (visual feedback)
- ✅ Point out "why" we're doing each step

**Don't:**
- ❌ Rush through code
- ❌ Hide the code (project clearly)
- ❌ Skip errors—show how to debug
- ❌ Assume they know Python
- ❌ Spend time on environment setup during session

### Handling Common Issues

**Problem: Notebook won't start**
```python
# Solution: Restart kernel
# Menu → Kernel → Restart & Clear Output
```

**Problem: Package import fails**
```bash
# Quick fix during workshop:
!pip install xgboost  # In Jupyter cell
```

**Problem: Participants falling behind**
- Reduce live coding time
- Provide pre-run notebooks with outputs
- Focus on explanation, not line-by-line coding

### Engagement Techniques

1. **Polls/Questions:**
   - "Before we build this model, what do you think will be the most important feature?"
   - "Why do you think LSTM would be better than regression for this?"

2. **Real-World Context:**
   - "This is exactly the problem Hochfrequenz clients face..."
   - "In production, this model runs 24/7, making thousands of predictions..."

3. **Hands-On Moments:**
   - "Now YOU adjust this hyperparameter and see what happens"
   - "Can anyone spot the bug in this code?"

---

## 📊 Notebook Structure & Customization

### Block 1: Regression (01_regression_forecasting.ipynb)

**Key Sections:**
1. Data loading & exploration (synthetic data)
2. Feature engineering
3. Baseline model (seasonal average)
4. Linear Regression
5. Random Forest
6. XGBoost
7. Model comparison
8. Feature importance

**Customization Options:**
- **Real data:** Replace synthetic data generator with Zenodo dataset
- **More features:** Add weather, price, holidays
- **Different use case:** Swap "household" for "DSO aggregate load"

### Block 2: LSTM (02_lstm_timeseries.ipynb)

**Key Sections:**
1. ANN fundamentals review
2. Why RNNs (sequence memory)
3. LSTM architecture deep-dive
4. Build LSTM in Keras
5. Train on time series
6. 24-hour forecast
7. Compare to Block 1 models

**Customization Options:**
- **Multi-step ahead:** Increase forecast horizon (1h → 6h → 24h)
- **Attention visualization:** Show which time steps matter most
- **Ensemble:** Combine LSTM + XGBoost predictions

### Block 3: Anomaly Detection (03_anomaly_detection.ipynb)

**Key Sections:**
1. ANN as building block
2. Encoder-Decoder architecture
3. Autoencoder loss function
4. Reconstruct normal patterns only
5. Detect anomalies
6. Case study: Synthetic fraud injection

**Customization Options:**
- **Real anomalies:** If you have labeled fraud data
- **Isolation Forest:** Simpler baseline
- **Seasonal decomposition:** Remove trend before anomaly detection

### Block 4: Integration (04_integrated_project.ipynb)

**Structured as:**
1. Define business problem
2. Data preparation (reuse from Block 1)
3. Run forecasting model (from Block 2)
4. Run anomaly detection (from Block 3)
5. Run customer segmentation (new: K-Means clustering)
6. Generate summary report
7. Discuss business impact

**Group Activity (Optional):**
- Pair participants
- Each pair solves mini version of Block 4
- Share results in 3-minute presentations

---

## 🔧 Technical Troubleshooting

### If Workshop Needs to Go Fully Remote

**Setup:**
- Zoom + Screen share
- Send participants notebooks in advance
- Have them download data beforehand
- Use Zoom whiteboard for architecture diagrams

**Adjustments:**
- Reduce live coding (pre-record if needed)
- Increase Q&A time
- Use breakout rooms for pair programming
- Record sessions for later reference

### If No Internet During Workshop

**Contingency:**
- Pre-download all datasets
- Have all packages pre-installed
- Use offline documentation (print cheat sheets)
- Have backup presentations (no web links)

### If GPU Not Available

**No problem!** 
- All notebooks work on CPU
- Block 3 & 4 might take slightly longer
- Modern CPUs are sufficient for this dataset size

---

## 📈 Assessment & Feedback

### During Workshop

**Checkpoint Questions:**
- Block 1: "Why does Random Forest outperform Linear Regression?"
- Block 2: "What does the 'memory' (cell state) in LSTM do?"
- Block 3: "How do we decide the anomaly threshold?"
- Block 4: "What would happen if we don't have labeled data?"

### After Workshop

**Feedback Survey (5 min):**

```
1. Pace: Too Fast / Just Right / Too Slow
2. Difficulty: Too Hard / Just Right / Too Easy
3. Most valuable block: [select one]
4. One thing to improve: [open text]
5. Will you apply this to projects? Yes / Maybe / No
6. Would recommend to colleagues? Yes / No
```

**Success Metrics:**
- 80%+ participants say "Just Right" pace
- 70%+ say they learned something valuable
- 50%+ say they'd apply to actual projects

---

## 🚀 Post-Workshop Follow-Up

### Week 1: Consolidate Learning

- [ ] Send recording + all notebooks to participants
- [ ] Share cheat sheets and additional resources
- [ ] Post Q&A answers in shared channel

### Week 2-4: Identify Projects

- [ ] Schedule 1:1 calls with interested participants
- [ ] Discuss client projects where ML could help
- [ ] Identify pilot projects to start

### Month 2+: Build Capability

- [ ] Monthly ML discussion group
- [ ] Advanced workshop (Transformers, MLOps)
- [ ] Internal blog posts on implementations

---

## 📚 Reference Materials to Print/Share

### Cheat Sheet 1: ML Model Selection
```
Use Regression if:
  - Linear relationship between features & target
  - Interpretability matters
  - Few features

Use Random Forest if:
  - Non-linear relationships
  - Don't care about interpretability
  - Medium-sized dataset

Use XGBoost if:
  - Want best performance
  - Have time to tune hyperparameters
  - Modern computational power available

Use LSTM if:
  - Time series data
  - Need to capture temporal dependencies
  - Have enough historical data

Use Autoencoder if:
  - Need to detect anomalies
  - Limited labeled data
  - Want unsupervised learning
```

### Cheat Sheet 2: Evaluation Metrics
```
Regression Metrics:
- MAE (Mean Absolute Error): Average prediction error in original units
- RMSE (Root Mean Squared Error): Penalizes larger errors more
- R² Score: Fraction of variance explained (0-1, higher is better)

Time Series Specific:
- MAPE (Mean Absolute Percentage Error): Useful when scale varies
- Forecast horizon error: Separate evaluation for different lead times

Anomaly Detection:
- Precision: Of detected anomalies, how many are real?
- Recall: Of real anomalies, how many did we catch?
- ROC-AUC: Trade-off between precision & recall
```

### Cheat Sheet 3: Deep Learning Architectures
```
ANN (Artificial Neural Network)
Input → Dense(ReLU) → Dense(ReLU) → Output
- Best for: Static data with non-linear patterns

RNN (Recurrent Neural Network)
Input → RNN → RNN → RNN → Output
- Best for: Sequences, but struggles with long-term dependencies

LSTM (Long Short-Term Memory)
Input → LSTM(memory) → LSTM(memory) → Output
- Best for: Time series with long-term patterns

Transformer
Input → Attention(all timesteps) → Dense → Output
- Best for: Long sequences, parallelizable
```

---

## 💡 Real Hochfrequenz Integration Ideas

### Possible Follow-On Projects

1. **Billing Optimization**
   - Train LSTM on customer consumption
   - Predict annual bill to reduce disputes
   - Segment customers for targeted programs

2. **Grid Operations**
   - Forecast peak load 24h ahead
   - Detect consumption anomalies (fraud/failure)
   - Route alerts to field teams

3. **Market Communication**
   - Predict market metering data accuracy
   - Detect data transmission failures
   - Anomaly detection on settlement data

4. **Process Automation**
   - Automate QA checks on consumption data
   - Predict processing failures before they happen
   - ML-based routing of customer requests

---

## 🎁 Deliverables for Participants

Each participant receives:

1. ✅ **Jupyter Notebooks** – All 4 blocks, fully executable
2. ✅ **Cheat Sheets** – PDF reference guides
3. ✅ **Data** – German smart meter dataset (or generation script)
4. ✅ **Presentation Slides** – Architecture diagrams & key concepts
5. ✅ **GitHub Repo** – Private or internal repo with all materials
6. ✅ **Certificate** – "Completed Hochfrequenz AI Workshop"

---

## ❓ FAQ for Facilitators

**Q: What if participants don't know Python?**
A: That's okay! Focus on concepts first, code second. Pair experienced developers with beginners during Block 4.

**Q: How long to prepare?**
A: 2-3 weeks for first-time delivery. Includes environment testing, running notebooks 3-4x, customizing for audience.

**Q: Can we do half-day?**
A: Not recommended—would cut content by 50%. Instead, do full-day with less depth on Block 4, or do 2-day workshop going deeper.

**Q: What's the max # of participants?**
A: 20-25 ideal. With TAs, can do 30-40 if you have enough compute resources.

**Q: Can we deliver this to clients?**
A: Yes! This is a great offering. Consider packaging it as:
- "AI for Energy" workshop (1 day)
- Custom version focused on their specific domain

---

## 📞 Support

**Before Workshop:**
- Reach out if environment issues
- Test your setup in advance
- Review notebooks thoroughly

**During Workshop:**
- Have a co-facilitator or TA
- Have their contact info handy
- Take notes on what works/doesn't

**After Workshop:**
- Gather feedback
- Identify follow-on projects
- Build internal community

---

**Good luck! This workshop has potential to transform how Hochfrequenz approaches consulting. 🚀**

---

*Last updated: December 5, 2024*  
*For questions: [facilitator email]*
