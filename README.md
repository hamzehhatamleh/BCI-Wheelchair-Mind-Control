# BCI Mind Control System (Graduate Project)

An assistive technology framework designed for individuals with severe paralysis or speech impairments. This Brain-Computer Interface (BCI) translates imagined speech navigation cues into motor execution signals, allowing users to control an automated wheelchair seamlessly using cognitive perception. 

The system maps thought patterns to 4 directional motions: **Up (Forward), Down (Backward), Left, and Right**.

---

## 🧠 System Architecture Overview

The system pipeline is split into three core phases: high-resolution data collection, optimized machine learning feature processing, and physical hardware control.

```text
  [ EMOTIV EEG Headset ] ---> [ Wavelet Decomposition (DWT) ] ---> [ Dimension Reduction (PCA) ] 
                                                                               │
  [ Raspberry Pi Chassis ] <-- [ GPIO Motor Driver Commands ] <--- [ PSO-Tuned KNN Classifier ]


1. Data Collection & Preprocessing
-Hardware: Brainwave telemetry is collected utilizing an advanced wireless EMOTIV EEG headset.

-Spatial Configuration: Raw data tracks electoral fluctuations across 5 critical cortical hubs mapping linguistic imagination and motor planning: EEG.AF3, EEG.T7, EEG.Pz, EEG.T8, and EEG.AF4.

-Preprocessing: Continuous timeseries signals are standardized via a rolling StandardScaler to mitigate artifact noises and physical drifts.



2. Feature Extraction & Machine Learning Pipeline
-Discrete Wavelet Transform (DWT): Raw temporal signals are split into localized frequency domains utilizing a 4-level Daubechies 2 (db2) wavelet decomposition. This extracts multi-resolution approximations and details (cA4, cD4, cD3, cD2, cD1) capturing specific neural rhythms corresponding to distinct imagined words.

-Dimensionality Reduction: High-dimension wavelet matrices are squeezed through Principal Component Analysis (PCA) to extract core mathematical variance components while maintaining high execution speeds.

-Classification Optimization: The core classifier utilizes K-Nearest Neighbors (KNN). To maximize reliability, a Particle Swarm Optimization (PSO) metaheuristic wrapper selects optimal parameters, establishing a robust testing accuracy of 80.24%.



3. Real-Time Hardware Execution
-Telemetry Evaluation: Incoming real-time streaming data matrices are continuously validated against a serialized model state (knn_model.joblib).

-Hardware Drivers: Multi-threaded micro-controllers evaluate statistical mode predictions on the edge via a Raspberry Pi.

-Actuation Logic: Classification labels map directly to low-level pulse-width modulated electrical signals transmitted over RPi.GPIO pins to standard dual-differential H-bridge motor drivers.


4. Repository Breakdown

├── Model/
│   └── final_GP2_code.ipynb      # Training sandbox: Features DWT extraction, PSO tuning, and model validation
└── Pipeline Code/
    ├── feature_extraction.py     # Functional script transforming raw telemetry data arrays into PCA vectors
    ├── test_model.py             # Inference pipeline evaluating localized CSV feeds against saved weights
    ├── knn_model.joblib          # Optimized serialized production model parameters
    └── imagined_BCI.py           # Core execution node binding inference inputs to RPi GPIO electrical pins
