Predictive Campus Life Wellness Sentinel

A machine learning-based early-warning system designed to identify declining student campus engagement and behavioral patterns by analyzing aggregated campus-life indicators such as facility usage, dining activity, event participation, social interactions, residence engagement, and trend metrics.

Note: This project uses synthetic data and is intended solely as a behavioral early-warning prototype. It is not a clinical mental-health diagnosis system.

📌 Overview

Educational institutions require proactive mechanisms to understand shifting student participation levels so that potential disengagement patterns can be identified early.

The Predictive Campus Life Wellness Sentinel provides an end-to-end prototype that:

Generates a synthetic multi-week student behavioral dataset.

Preprocesses features and computes dynamic, trend-based indicators.

Trains and compares Random Forest and XGBoost classification models.

Gives particular attention to High-risk recall during model comparison.

Serializes the selected Random Forest model for reuse.

Tests the pretrained model using new student profiles.

Exposes predictions through a Flask REST API.

Provides an interactive Streamlit interface for risk prediction.

Publishes prediction events to RabbitMQ.

Consumes and processes RabbitMQ messages using a separate consumer notebook.

🎯 Objectives

Dataset Generation: Produce realistic, multi-week synthetic student behavioral data.

Feature Engineering: Build trend-based features such as percentage changes and rolling engagement.

Risk Classification: Classify student risk into Low (0), Medium (1), and High (2).

Model Comparison: Compare Random Forest and XGBoost performance.

High-risk Detection: Give particular attention to High-risk recall for this early-warning use case.

Model Reusability: Save the trained model so it can be reused without retraining.

Independent Testing: Verify predictions using the pretrained model.

API Integration: Provide predictions through a Flask REST API.

User Interface: Provide an interactive Streamlit interface connected to the API.

Messaging Integration: Publish prediction results to RabbitMQ and process them through a consumer.

Modular Architecture: Keep dataset generation, training, testing, API, and messaging components in separate notebooks.

🏗️ Project Architecture

                    ┌────────────────────────────┐
                    │   Synthetic Dataset        │
                    │   Generation Notebook      │
                    └──────────────┬─────────────┘
                                   │
                                   ▼
                    ┌────────────────────────────┐
                    │ Data Preprocessing &        │
                    │ Feature Engineering         │
                    └──────────────┬─────────────┘
                                   │
                                   ▼
                    ┌────────────────────────────┐
                    │ Model Training Notebook    │
                    │                            │
                    │ • Random Forest            │
                    │ • XGBoost                  │
                    │ • Evaluation               │
                    └──────────────┬─────────────┘
                                   │
                                   ▼
                    ┌────────────────────────────┐
                    │  Trained Random Forest     │
                    │  Model (.pkl)              │
                    └──────────────┬─────────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    ▼                             ▼
          ┌──────────────────┐          ┌──────────────────┐
          │ Model Testing    │          │ Flask REST API   │
          │ Notebook         │          │ /predict         │
          └──────────────────┘          └────────┬─────────┘
                                                 │
                                  ┌──────────────┴──────────────┐
                                  ▼                             ▼
                       ┌──────────────────┐          ┌────────────────────┐
                       │ Streamlit        │          │ RabbitMQ Producer  │
                       │ Interface        │          │ Prediction Event   │
                       └──────────────────┘          └─────────┬──────────┘
                                                               │
                                                               ▼
                                                   ┌────────────────────────┐
                                                   │ RabbitMQ Queue         │
                                                   │ student_wellness_      │
                                                   │ predictions             │
                                                   └───────────┬────────────┘
                                                               │
                                                               ▼
                                                   ┌────────────────────────┐
                                                   │ RabbitMQ Consumer       │
                                                   │ Message Processing      │
                                                   └────────────────────────┘

📂 Project Structure

Predictive_Campus_Life_Wellness_Sentinel/
│
├── Dataset/
│   └── student_wellness_prediction_dataset.csv
│
├── Models/
│   └── wellness_randomforest_pipeline.pkl
│
├── Notebooks/
│   ├── dataset_generation.ipynb
│   ├── RandomForest_train.ipynb
│   ├── RandomForest_test.ipynb
│   ├── RandomForest_API.ipynb
│   └── RabbitMQ_Consumer.ipynb
│
└── README.md

📓 Google Colab Notebooks

1. Data Generation & Preprocessing

Notebook: dataset_generation.ipynb

This notebook generates and prepares the synthetic student behavioral dataset.

Main Tasks

Generate synthetic multi-week student behavioral data.

Perform preprocessing.

Create behavioral and engagement features.

Generate risk labels.

Save the final dataset.

Download the dataset for reuse.

Dataset File

student_wellness_prediction_dataset.csv

Notebook

Open Dataset Generation Notebook in Google Colab

📊 Dataset & Feature Engineering

The dataset represents student campus-life behavioral and engagement patterns across multiple weeks.

Key Features

Feature Name

Description

facility_usage

Frequency of campus facility utilization

dining_activity

Dining/cafeteria activity

event_participation

Participation in campus events and activities

club_participation

Engagement in student clubs and organizations

residence_engagement

Engagement within the residence environment

recreation_activity

Recreational activity level

social_interactions

Peer-to-peer interaction indicator

communication_activity

Campus communication and interaction frequency

sleep_quality

Behavioral sleep-quality indicator

academic_engagement

Academic participation and engagement

campus_engagement_score

Composite campus engagement indicator

social_isolation_score

Behavioral social-isolation indicator

engagement_change_pct

Percentage change in engagement from the previous period

rolling_3_week_engagement

Smoothed engagement across three weeks

Target Variable (risk_label)

0 → Low Risk
1 → Medium Risk
2 → High Risk

Feature engineering was performed to capture behavioral trends rather than only static values.

Campus Engagement Score summarizes overall campus participation.

Social Isolation Score combines social and interaction-related indicators.

Engagement Change Percentage captures increasing or declining engagement.

Rolling 3-Week Engagement helps identify sustained changes and reduce temporary fluctuations.

🤖 2. Model Training, Evaluation & Serialization

Notebook: RandomForest_train.ipynb

This notebook loads the generated dataset and performs the complete model-training workflow.

Training Workflow

Load Dataset
      ↓
Data Validation
      ↓
Feature Selection
      ↓
Student-Level Train/Test Split
      ↓
Random Forest Training
      ↓
XGBoost Training
      ↓
Model Evaluation
      ↓
Model Comparison
      ↓
Random Forest Selection
      ↓
Model Serialization

Train/Test Strategy

Split Ratio: 80% Training / 20% Testing

Grouping: Student-level split to reduce data leakage between training and testing.

Models Used

Random Forest: An ensemble learning algorithm that combines multiple decision trees for classification.

XGBoost: A gradient-boosting algorithm used as a comparison model.

Evaluation Metrics

Accuracy

Precision

Recall

F1-score

Confusion Matrix

Model Comparison

Because this is an early-warning use case, High-risk recall was given particular attention.

In the experiment conducted on the synthetic test dataset:

Model

High-risk Recall

Status

Random Forest

~40%

Selected

XGBoost

~17%

Evaluated

Based on this experiment, Random Forest was selected for the subsequent testing and API stages.

Note: These results are specific to the synthetic dataset used for this prototype and serve as a proof of concept.

Saved Model

wellness_randomforest_pipeline.pkl

Notebook

Open Model Training Notebook in Google Colab

🧪 3. Pretrained Model Inference & Testing

Notebook: RandomForest_test.ipynb

This notebook independently tests the previously trained Random Forest model without retraining it.

Testing Workflow

Load Pretrained Model
        ↓
Prepare New Student Profile
        ↓
Prepare Input Features
        ↓
Run Prediction
        ↓
Generate Probability Scores
        ↓
Validate Output

The saved model is loaded from:

wellness_randomforest_pipeline.pkl

New synthetic student behavioral profiles are provided to the model.

For each profile, the model generates:

Predicted risk level

Low probability

Medium probability

High probability

Example

Predicted Risk: Medium

Low:     1.18%
Medium:  62.01%
High:    36.81%

Multiple profiles were tested to verify that the pretrained model could perform predictions successfully without retraining.

Notebook

Open Model Testing Notebook in Google Colab

🌐 4. Flask REST API & Streamlit Integration

Notebook: RandomForest_API.ipynb

This notebook integrates the pretrained Random Forest model with a Flask REST API, a Streamlit interface, and the RabbitMQ producer.

API Workflow

Student Behavioral Data
          │
          ▼
     JSON Request
          │
          ▼
     Flask /predict
          │
          ▼
Pretrained Random Forest
          │
          ▼
    Risk Prediction
          │
          ▼
  Probability Scores
          │
          ▼
    RabbitMQ Producer

🔌 Flask REST API

Prediction Endpoint

POST /predict

The endpoint accepts student behavioral data in JSON format and returns the predicted risk level and probability distribution.

Request Body Example

{
  "student_id": 1001,
  "facility_usage": 45,
  "dining_activity": 50,
  "event_participation": 30,
  "club_participation": 25,
  "residence_engagement": 40,
  "recreation_activity": 35,
  "social_interactions": 30,
  "communication_activity": 40,
  "sleep_quality": 60,
  "academic_engagement": 55,
  "campus_engagement_score": 42,
  "social_isolation_score": 58,
  "engagement_change_pct": -12,
  "rolling_3_week_engagement": 48
}

student_id is carried as message metadata and is not one of the 14 machine-learning input features.

Response Example

{
  "student_id": 1001,
  "predicted_risk": "Medium",
  "probabilities": {
    "Low": 0.23,
    "Medium": 70.53,
    "High": 29.25
  },
  "rabbitmq": {
    "published": true,
    "queue": "student_wellness_predictions"
  }
}

The API was successfully tested with an HTTP 200 response.

API Authentication

No external API key is required for this prototype.

The Flask API is created and run locally inside the Google Colab environment and directly uses the locally loaded pretrained Random Forest model.

📨 5. RabbitMQ Messaging Integration

Producer: RandomForest_API.ipynb
Consumer: RabbitMQ_Consumer.ipynb

RabbitMQ was integrated to create an asynchronous messaging flow for ML prediction events.

RabbitMQ Configuration

The development RabbitMQ instance was configured using:

Host     : 129.153.75.221
Port     : 5672
Username : bytesmart_interns
VHost    : /
Queue    : student_wellness_predictions
Protocol : AMQP

Security: Credentials are entered securely at runtime and should not be committed to the repository or README.

Producer

The Flask /predict endpoint acts as the RabbitMQ producer.

After the Random Forest generates a prediction, the API prepares a JSON message containing:

Student ID

Predicted risk

Low probability

Medium probability

High probability

The message is published to:

student_wellness_predictions

Producer Message Example

{
  "student_id": 1001,
  "predicted_risk": "Medium",
  "probabilities": {
    "Low": 0.23,
    "Medium": 70.53,
    "High": 29.25
  }
}

Consumer

A separate RabbitMQ_Consumer.ipynb notebook connects to the same queue and waits for messages.

The consumer:

Receives the RabbitMQ message.

Decodes the JSON payload.

Extracts the student ID and risk prediction.

Processes the prediction.

Displays an appropriate processing action.

Sends an acknowledgement after successful processing.

Consumer Processing Logic

Message Received
       ↓
Decode JSON
       ↓
Extract Prediction
       ↓
Process Message
       ↓
Successful?
   /          YES          NO
  ↓            ↓
ACK        NACK / Reject

Example processing:

High Risk   → Flag for early intervention review
Medium Risk → Continue monitoring
Low Risk    → No immediate action required

These actions are prototype processing messages and are not intended to make clinical or disciplinary decisions.

End-to-End Messaging Flow

Flask /predict
      ↓
Random Forest
      ↓
Prediction Generated
      ↓
RabbitMQ Producer
      ↓
student_wellness_predictions
      ↓
RabbitMQ Consumer
      ↓
Message Processing
      ↓
Message ACK

The Producer → RabbitMQ → Consumer workflow was successfully tested end-to-end.

🖥️ Streamlit Interface

The Streamlit interface provides an interactive way to enter student behavioral information and view the model prediction.

Streamlit Workflow

User Enters Student Data
          ↓
Streamlit Interface
          ↓
Flask /predict API
          ↓
Random Forest Model
          ↓
Prediction + Probabilities
          ↓
Streamlit Result Display

The interface contains input fields for all 14 behavioral and engagement features.

The results displayed include:

Predicted risk level

Low probability

Medium probability

High probability

Probability distribution chart

Notebook

Open Flask API & Streamlit Notebook in Google Colab

🔄 Phase 3 End-to-End Workflow

Phase 3 adds asynchronous messaging to the prediction application.

             ┌──────────────────────┐
             │ Student Behavioral   │
             │ JSON Request          │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ Flask /predict       │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ Random Forest Model  │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ Risk + Probabilities │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ RabbitMQ Producer    │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ RabbitMQ Queue       │
             │ student_wellness_    │
             │ predictions          │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ RabbitMQ Consumer    │
             └──────────┬───────────┘
                        │
                        ▼
             ┌──────────────────────┐
             │ Process Message      │
             │ + ACK                │
             └──────────────────────┘

🛠️ Tech Stack

Domain

Technologies

Programming Language

Python 3

Data Processing

Pandas, NumPy

Machine Learning

Scikit-learn, XGBoost

Model

Random Forest Classifier

Model Serialization

Joblib

Visualization

Matplotlib

Backend / API

Flask, Flask-CORS

API Communication

Requests

Frontend UI

Streamlit

Messaging

RabbitMQ

RabbitMQ Client

Pika

Environment

Google Colab, Jupyter Notebooks

🚀 Execution Order

Run the notebooks in the following order:

1. Notebooks/dataset_generation.ipynb
        ↓
2. Notebooks/RandomForest_train.ipynb
        ↓
3. Notebooks/RandomForest_test.ipynb
        ↓
4. Notebooks/RandomForest_API.ipynb
        ↓
5. Notebooks/RabbitMQ_Consumer.ipynb

For the messaging demonstration, the consumer should be running before sending a new prediction request so that the published event can be observed immediately.

Notebook 1

Generates:

Dataset/student_wellness_prediction_dataset.csv

Notebook 2

Trains the models and generates:

Models/wellness_randomforest_pipeline.pkl

Notebook 3

Loads the .pkl file independently and verifies inference on new student profiles.

Notebook 4

Loads the pretrained model, launches the Flask REST API, publishes prediction events to RabbitMQ, and provides the Streamlit interface.

Notebook 5

Connects to RabbitMQ, consumes prediction events, processes the messages, and acknowledges successful processing.

🧩 Project Challenges & Solutions

Challenge

Solution

Real student behavioral data was unavailable

Created a synthetic dataset for prototype development

Student behavior changes over time

Created engagement-change and rolling-engagement features

Temporary behavioral changes can affect predictions

Used Rolling 3-Week Engagement

High-risk class is important for early warning

Focused on High-risk recall during model comparison

Random Forest and XGBoost produced different results

Compared both models using classification metrics

Model should not be retrained for every prediction

Serialized the trained Random Forest model using Joblib

Need to verify the saved model independently

Created a separate pretrained model testing notebook

Need an application interface

Created Flask REST API and Streamlit interface

Flask /predict route was registered more than once during development

Restarted the Colab runtime and maintained a single /predict endpoint

Flask port 5000 was already in use

Restarted the runtime to clear the previous Flask process

RabbitMQ connection was reset during API execution

Re-established the RabbitMQ connection and recreated the channel

Need asynchronous prediction event handling

Implemented RabbitMQ producer and separate consumer

Consumer needed to continuously listen for messages

Created a separate consumer notebook using start_consuming()

Messages must not be lost after successful processing

Used durable queue configuration and explicit message acknowledgement

RabbitMQ credentials should not be exposed

Used secure runtime password input instead of hard-coding the password

📈 Key Features

✅ Synthetic multi-week student behavioral dataset

✅ Data preprocessing

✅ Behavioral feature engineering

✅ Campus engagement score

✅ Social isolation score

✅ Engagement change percentage

✅ Rolling 3-week engagement

✅ Low / Medium / High risk classification

✅ Random Forest classification

✅ XGBoost model comparison

✅ High-risk recall analysis

✅ Model serialization using .pkl

✅ Pretrained model inference

✅ Independent model testing

✅ Flask REST API

✅ /predict endpoint

✅ JSON input and output

✅ Streamlit interface

✅ Probability-based predictions

✅ Probability distribution visualization

✅ RabbitMQ producer

✅ RabbitMQ queue

✅ RabbitMQ consumer

✅ JSON message processing

✅ Message acknowledgement

✅ End-to-end Producer → RabbitMQ → Consumer flow

✅ Modular Google Colab notebooks

✅ Structured project folder

✅ Documented and commented code

⚠️ Disclaimer & Ethical Considerations

Synthetic Data: The system was trained and evaluated using synthetically generated data.

Non-Clinical: The system provides behavioral engagement risk indicators only. It is not a clinical diagnostic or mental-health assessment tool.

Prototype: The current system is intended for educational, research, internship, and prototype demonstration purposes.

Real-World Deployment: Before real-world use, the system would require appropriate validation using representative institutional data, privacy protections, ethical safeguards, bias evaluation, monitoring, and qualified human oversight.

Human Decision-Making: Predictions should be treated as indicators for further review and should not be used as the sole basis for decisions concerning individual students.

Data Privacy: Real student data should only be processed using appropriate institutional authorization, privacy controls, and security practices.

Credential Security: RabbitMQ credentials must be kept outside source code and documentation.

📚 Project Resources

Google Colab Notebooks

Dataset Generation

Open Notebook

Model Training

Open Notebook

Model Testing

Open Notebook

Flask API & Streamlit

Open Notebook

RabbitMQ Consumer

Add the Google Colab link for RabbitMQ_Consumer.ipynb after sharing the notebook.

📄 License

This project is intended for educational, research, internship, and prototype development purposes.
