# Handson-L10-Spark-Streaming-MachineLearning-MLlib

## Overview
This project implements a comprehensive real-time analytics pipeline for a ride-sharing platform using Apache Spark Structured Streaming and MLlib. The system simulates real-time ride data, processes streaming data, and implements machine learning algorithms for fare prediction and trend analysis.

### Learning Objectives
- Implement real-time data streaming with Apache Spark
- Train and deploy ML models for regression tasks
- Process streaming data with windowed aggregations
- Engineer time-based features for time series prediction
- Perform real-time inference on streaming data

---

## Project Structure
```
Handson-L10-Spark-Streaming-MachineLearning-MLlib-main/
├── models/
|   ├── fare_model
|   └── fare_trend_model_v2
├── task4.py
├── task5.py
├── Output1
├── Output2
├── Output3
├── training-dataset.csv
├── data_generator.py
├── L10_itcs6190_Spark_Streaming With Mlib
└── README.md
```

---

## Requirements

Before running the project, make sure you have:

- Python 3 installed  
- PySpark installed  
- Faker library installed  
- Java (version 11 or 17 works best with Spark)

Install required Python packages:
```bash
pip install pyspark faker
```
---

## How to Run

### 1. Start the Data Generator
First, run the script that generates streaming ride data:
```bash
python3 data_generator.py
```
This will continuously send JSON data to `localhost:9999`.

---

### 2. Task 4:  Basic Streaming Ingestion and Parsing

### Instructions

1. **Load the training data** from `training-dataset.csv`
2. **Use VectorAssembler** to prepare the feature column (`distance_km`)
3. **Create and fit a LinearRegression model** to the training data
4. **Save the trained model** to `models/fare_model`
5. **In streaming logic, load the saved model**
6. **Apply the model** to generate a `prediction` column
7. **Compute deviation** = |fare_amount - prediction|
8. **Print results** to console using `outputMode("append")`

---

## Explanations

### Spark Structured Streaming
Scalable stream processing engine built on Spark SQL. Provides exactly-once processing and allows streaming computations to be expressed like batch queries.

### MLlib for Real-Time Predictions
- Train models on historical data with distributed computing
- Load models into streaming pipelines for real-time inference
- Handle millions of predictions per second

### Key Concepts

**Linear Regression**: Models relationship between fare_amount (dependent) and distance_km (independent):

```bash
python3 task4.py
```
What it does:
- Reads data from the socket
- Parses JSON into columns
- Displays the data in the console

## Task 5: Time-Based Fare Trend Prediction

### Instructions

1. **Load the training data** from `training-dataset.csv`
2. **Pre-process** by grouping into 5-minute windows and calculating `avg_fare`
3. **Create features** `hour_of_day` and `minute_of_hour` from `window.start` time
4. **Train Linear Regression model** on these features and save to `models/fare_trend_model_v2`
5. **Apply same windowed aggregation** to streaming data
6. **Load saved model** and apply to aggregated streaming DataFrame
7. **Print** `window_start`, `window_end`, actual `avg_fare`, and `predicted_next_avg_fare`

---

## Explanations

### Time-Based Feature Engineering
Instead of using raw timestamps, cyclical features capture time patterns:
- **hour_of_day (0-23)**: Captures daily patterns (rush hours, late night)
- **minute_of_hour (0-59)**: Captures sub-hour patterns (15-minute intervals)

```bash
python3 task5.py
```
What it does:
- **Creates time features** (hour, minute) from timestamps
- **Groups data into 5-minute windows** and averages fares
- **Trains model** on time features to predict fare trends
- **Streams live data** into same 5-minute windows
- **Forecasts next window's average fare** in real-time
- **Shows actual vs predicted** window averages on console

## Sample Outputs for Task4 and Task5 
<img width="1920" height="1080" alt="Output1" src="https://github.com/user-attachments/assets/86aa97d8-b783-411a-bdcf-bda657eb6f38" />
<img width="1920" height="1080" alt="Output2" src="https://github.com/user-attachments/assets/db48e1bf-8acd-4863-86df-e7102b705ff8" />
<img width="1920" height="1080" alt="Output3" src="https://github.com/user-attachments/assets/ce659ed2-d67a-401a-9b93-301abd764cb0" />

## Conclusion

This hands-on project successfully demonstrates how to build a real-time analytics pipeline using Apache Spark Structured Streaming and MLlib. Key takeaways include:

- **Offline training with streaming inference** provides the best of both worlds—accurate models from historical data with real-time predictions on live streams
- **Feature engineering matters**—time-based cyclical features (hour, minute) capture patterns that raw timestamps cannot
- **Windowed aggregations** reduce noise and reveal meaningful trends while enabling efficient stream processing
- **MLlib integration** with Structured Streaming is seamless—models trained once can be loaded and applied to streaming DataFrames with minimal code
- **Anomaly detection** through deviation calculation helps identify unusual fare patterns in real-time
- **Watermarks** are essential for handling late-arriving data in production streaming environments
