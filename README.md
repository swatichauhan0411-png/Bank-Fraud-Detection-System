# Bank-Fraud-Detection-System
Bank Fraud Detection using Machine Learning
## 📌 Project Overview
This project is a Machine Learning based Bank Fraud Detection System. It analyzes transaction details and predicts whether a transaction is **Normal** or **Fraudulent**.
The project uses a **Random Forest Classifier** trained on the PaySim financial transaction dataset.
## 🎯 ObjectiveThe main objective of this project is to detect potentially fraudulent financial transactions using Machine Learning.
## 🧠 Machine Learning Model
The project uses:
* Random Forest Classifier
* Scikit-learn
* Pandas
* NumPy
* Joblib
The dataset contains transaction information such as:
* Transaction type
* Transaction amount
* Origin account balance
* Destination account balance
* Transaction step
* Fraud indicator
## ⚙️ Project Workflow
```textTransaction Input       ↓Data Preprocessing       ↓Feature Encoding       ↓Random Forest Model       ↓Fraud Prediction       ↓Normal / Fraudulent Result```
## 🌐 Web Application
The project uses **Flask** to create a web interface where users can enter transaction details.The application provides two possible outputs:
### ✅ Normal Transaction
The model predicts that the given transaction does not belong to the fraudulent class.
### ⚠️ Fraudulent Transaction DetectedThe model predicts that the given transaction belongs to the fraudulent class.
## 📊 Dataset
The project uses the PaySim synthetic financial transaction dataset.
The dataset contains more than **6.3 million transactions** and includes both normal and fraudulent transactions.
## 📈 Model PerformanceThe Random Forest model achieved approximately:
* ROC-AUC: **0.9935**
* Fraud Recall: **0.78**
* Fraud Precision: **0.98**
These results are based on the test data used during model evaluation.
## 📁 Project Structure
```textprojectmini/│├── data.csv├── app.py├── train_model.py├── requirements.txt├── README.md│├── model/│   └── fraud_model.pkl│├── templates/│   └── index.html│├── static/│   └── style.css│└── notebooks/    └── Analysis.ipynb```
## ▶️ How to Run
### 1. Activate virtual environment
```bashvenv\Scripts\activate```
### 2. Install dependencies
```bashpip install -r requirements.txt```
### 3. Run the Flask application
```bashpython app.py```
### 4. Open in browser
```texthttp://127.0.0.1:5000```
## ⚠️ Important Note
This project is an academic Machine Learning demonstration using a synthetic dataset. A prediction from this model should not be treated as proof that a real-world transaction is fraudulent.
## 👨‍💻 Project Purpose
This project demonstrates the practical use of:
* Machine Learning
* Classification
* Random Forest
* Class imbalance handling
* Feature preprocessing
* Flask web development
* Model deployment
Collapse file‎app.py‎Copy file name to clipboard+70Lines changed: 70 additions & 0 deletionsOriginal file line numberDiff line numberDiff line change@@ -0,0 +1,70 @@from flask import Flask, render_template, requestimport joblibimport pandas as pdapp = Flask(__name__)# Load trained modelmodel = joblib.load("model/fraud_model.pkl")@app.route("/")def home():    return render_template("index.html")@app.route("/predict", methods=["POST"])def predict():    step = float(request.form["step"])    transaction_type = request.form["type"]    amount = float(request.form["amount"])    oldbalanceOrg = float(request.form["oldbalanceOrg"])    newbalanceOrig = float(request.form["newbalanceOrig"])    oldbalanceDest = float(request.form["oldbalanceDest"])    newbalanceDest = float(request.form["newbalanceDest"])    isFlaggedFraud = int(request.form["isFlaggedFraud"])    type_CASH_OUT = 1 if transaction_type == "CASH_OUT" else 0    type_DEBIT = 1 if transaction_type == "DEBIT" else 0    type_PAYMENT = 1 if transaction_type == "PAYMENT" else 0    type_TRANSFER = 1 if transaction_type == "TRANSFER" else 0    data = pd.DataFrame([[        step,        amount,        oldbalanceOrg,        newbalanceOrig,        oldbalanceDest,        newbalanceDest,        isFlaggedFraud,        type_CASH_OUT,        type_DEBIT,        type_PAYMENT,        type_TRANSFER    ]], columns=[        "step",        "amount",        "oldbalanceOrg",        "newbalanceOrig",        "oldbalanceDest",        "newbalanceDest",        "isFlaggedFraud",        "type_CASH_OUT",        "type_DEBIT",        "type_PAYMENT",        "type_TRANSFER"    ])    prediction = model.predict(data)[0]    if prediction == 1:        result = "⚠️ Fraudulent Transaction Detected"    else:        result = "✅ Normal Transaction"    return render_template("index.html", prediction=result)if __name__ == "__main__":    app.run(debug=True)
