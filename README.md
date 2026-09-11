# Driver Drowsiness Detection Using Real-Time Heart Rate Variability

A research implementation for detecting **driver drowsiness from heart rate variability (HRV)** using an Artificial Neural Network (ANN). The project processes pulse-rate data, derives time-domain, frequency-domain, and non-linear HRV features, and classifies drivers into **Alert**, **Early Drowsiness**, and **Severe Drowsiness** states.

> Publication: **Riadul Islam Rabbi, Em Poh Ping, and Jakir Hossen, “Driver Drowsiness Detection Using Real-Time Heart Rate Variability Data,” 2025 IEEE 15th Symposium on Computer Applications & Industrial Electronics (ISCAIE), pp. 320–325.**  
> DOI: https://doi.org/10.1109/ISCAIE64985.2025.11081139


## Research highlights

| Item | Description |
|---|---|
| Research problem | Non-intrusive, real-time driver drowsiness detection |
| Physiological signal | PPG-derived heart rate / HRV |
| Wearable device in the study | Empatica EmbracePlus |
| Participants | 9 |
| Driving environments | Urban, rural, and highway routes |
| Total reported instances | 807 |
| Reported class distribution | 96 Alert, 544 Early Drowsiness, 167 Severe Drowsiness |
| Model | Feed-forward ANN: 64 → 32 → 3 |
| Hidden activation | ReLU |
| Output activation | Softmax |
| Optimizer | Adam |
| Loss | Sparse categorical cross-entropy |
| Published accuracy | **91.36%** |
| Published macro precision | **85.33%** |
| Published macro recall | **82.33%** |
| Published macro F1-score | **83.33%** |

## Why this project matters

Drowsy driving reduces alertness and reaction ability and is an important road-safety problem. Camera-based and intrusive physiological systems can be sensitive to environmental conditions or inconvenient for drivers. This project investigates HRV as a wearable, non-intrusive signal for recognizing changes associated with drowsiness.

The implementation follows the research workflow from pulse-rate preprocessing to HRV feature extraction and ANN classification.

## Methodology

The pipeline contains five main stages:

1. **Data loading and cleaning** — read pulse-rate CSV files, validate pulse-rate values, and calculate RR intervals.
2. **HRV feature engineering** — extract time-domain, frequency-domain, and non-linear features.
3. **Label preparation** — use independent ground-truth labels when available, or optionally reproduce the threshold-based labels from the original implementation notebook.
4. **ANN training** — train a 64-neuron / 32-neuron hidden architecture with a 3-class softmax output.
5. **Evaluation** — report accuracy, macro precision, macro recall, macro F1-score, confusion matrix, and training curves.

### HRV features

**Time domain**

`mean_nni`, `sdnn`, `sdsd`, `nni_50`, `pnni_50`, `nni_20`, `pnni_20`, `rmssd`, `median_nni`, `range_nni`, `cvsd`, `cvnni`, `mean_hr`, `max_hr`, `std_hr`

**Frequency domain**

`lf_power`, `hf_power`, `lf_hf_ratio`

**Non-linear domain**

`csi`, `cvi`, `modified_csi`, `sampen`

## Repository structure

```text
.
├── driver_drowsiness_hrv_ann.ipynb
├── README.md                     
└── assets/                      
    ├── power spectral density of hrv.png
    ├── training and validation loss.png
    ├── training and validation accuracy.png
    ├── model performance metrics
    └── confusion metrix
```



## Labeling note

The supplied implementation notebook creates drowsiness labels using thresholds based on `RMSSD`, `LF power`, `HF power`, and `sample entropy`. The paper also reports use of the **Karolinska Sleepiness Scale (KSS)** for labeling.

For a rigorous predictive study, independently collected labels such as KSS should be preferred. Because the supplied paper text does not provide numeric KSS cutoffs for converting KSS values into the three classes, this repository does **not** invent those cutoffs.

The notebook therefore supports:

- `LABEL_MODE = "ground_truth"` — recommended when a valid `state` label is available.
- `LABEL_MODE = "heuristic"` — reproduces the original threshold-based implementation workflow.

## Reproducibility improvements in this GitHub version

The original analysis was useful for developing the research pipeline. This portfolio version improves the code quality by:

- removing hard-coded Google Colab paths;
- organizing feature extraction into reusable functions;
- keeping rolling windows within participant/session boundaries;
- fitting missing-value imputation and scaling only on the training data;
- separating validation data from the final test set;
- using stratified data splits;
- applying class weights to address the reported class imbalance;
- setting random seeds for reproducibility;
- saving the trained model and preprocessing objects;
- clearly separating published results from results produced by a new run.

Because of these improvements, a new run is not expected to reproduce the published values exactly.

## Results and Visualizations

This section can be used in GitHub to clearly **show the output of the project**.

### 1. Power Spectral Density of HRV

This visualization shows the **Power Spectral Density (PSD)** of HRV, highlighting the **Low Frequency (LF)** band and **High Frequency (HF)** band. These frequency-domain components help describe autonomic nervous system activity and are important for drowsiness analysis.

![Power Spectral Density of HRV](assets/power%20spectral%20density%20of%20hrv.png)

---

### 2. Training and Validation Accuracy

This plot shows how model accuracy improved over training epochs. Both training and validation accuracy increase steadily, suggesting that the model learned useful patterns and generalized reasonably well.

![Training and Validation Accuracy](assets/training%20and%20validation%20accuracy.png)

---

### 3. Training and Validation Loss

This plot shows the decrease in loss across epochs. The downward trend in both training and validation loss indicates improvement in model performance during training.

![Training and Validation Loss](assets/training%20and%20validation%20loss.png)

---

### 4. Model Performance Metrics

This bar chart summarizes the final reported performance of the ANN model.

![Model Performance Metrics](assets/model%20performance%20metrics.png)

### 5. Confusion Matrix

The confusion matrix shows the classification performance for the three driver states: **Alert**, **Early Drowsiness**, and **Severe Drowsiness**.

![Confusion Matrix](assets/confusion%20matrix.png)

## Published results

| Metric | Score |
|---|---:|
| Accuracy | **91.36%** |
| Precision (macro) | **85.33%** |
| Recall (macro) | **82.33%** |
| F1-score (macro) | **83.33%** |

The study reports strong recognition of early drowsiness, while some confusion remains between Alert/Early Drowsiness and Early/Severe Drowsiness.

### Interpretation of the output

- The model achieved **high overall accuracy**, indicating strong classification performance on the reported dataset.
- **Precision** shows that predicted drowsiness classes were mostly correct.
- **Recall** shows the model captured most true drowsiness cases, although some misclassification remained.
- **F1-score** indicates a balanced tradeoff between precision and recall.
- The confusion matrix suggests the model was strongest at detecting **Early Drowsiness**, which is useful because early intervention can improve road safety.
## Potential extensions

Future work can strengthen the project by using participant-wise cross-validation, larger and more balanced datasets, additional physiological signals, sequential models such as LSTM/GRU, and a real-time inference application.


## Data and ethics

Data available on request due to privacy/ethical restrictions. If the dataset cannot be shared, keep the `data/` directory out of Git and provide only the code, data schema, and instructions for authorized users.

## Citation

If you use or discuss this implementation, cite the associated publication:

```text
R. I. Rabbi, E. P. Ping, and J. Hossen,
"Driver Drowsiness Detection Using Real-Time Heart Rate Variability Data,"
2025 IEEE 15th Symposium on Computer Applications & Industrial Electronics (ISCAIE),
pp. 320–325, 2025.
doi: 10.1109/ISCAIE64985.2025.11081139
```

## Keywords

`driver drowsiness` `heart rate variability` `HRV` `PPG` `wearable computing` `deep learning` `artificial neural network` `road safety` `physiological signal processing`
