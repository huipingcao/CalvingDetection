# CalvingDetection
This github repository keeps the code and data for calving detection. 


This project focuses on detecting calving behavior in cattle using sensor data. It is divided into two main stages:

---

## Requirements
- Python 3.x
- Libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `joblib`, `seaborn`, `dateutil`, `torch`

---

## Stage 1: Anomaly Detection
First change all the file paths marked with
**# Change Required  #**

### Workflow
#### Training autoencoder
1. **Load and parse CSV files (Processed_data)** from the Processed_data folder (one file per cow).
2. **Compute features**:
   - `KNN_Sum`: sum of KNN2–KNN5
   - `ts`: derived behavioral index using `ActivityCount_Difference`, `Distance_moved`, and `KNN_Sum`
   - `sin_hour`: $np.sin(2 * pi * hours / 24)$
   - `cos_hour`: $np.cos(2 * pi * hours / 24)$
3. **Drop null entries** and set `Rounded_Time` as the index.
4. **Remove outliers** using IQR filtering on each feature.
5. **Normalize** the data using `MinMaxScaler` (individually per cow).
6. **Save** the fitted scaler for future use.
7. **Train** the autoencoder model.
#### Applying the trained autoencoder
8. **Process test data**.
    - Compute features needed
    - Drop null entries
    - Normalize the test data using saved `MinMaxScaler` (individually per cow).
9. **Apply the trained AE model** to detect anomalies.
#### Combined with the random forest model obtained in stage 2

### Output
- Individual Min-Max scaler files for each cow
- Trained autoencoder model
- Anomalies

---

## Stage 2: Classification
First change all the file paths marked with
**# Change Required  #**

### Workflow
1. **Load detected anomaly CSVs** for each cow.
2. **Add sliding features** including (MA, STD, Diff1, and Diff3) for each feature.
   - Moving average (MA) and standard deviation (STD) over 6 and 12 hours
   - Difference from previous hours (1 and 3 hours)
   - Applied on features: KNN2–KNN5, Distance_moved, ActivityCount_Difference
3. **Label anomalies** with boolean value to mark if it's a calving event.
4. **Train Random Forest model**


### Output
- Trained Random Forest model

---

