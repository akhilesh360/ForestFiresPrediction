# Algeria Wild Fires Prediction

This project predicts the risk of wildfires in Algeria using various environmental factors. The application is built using Flask and deployed on AWS Elastic Beanstalk.
Link: http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/

## Project Overview

The project involves the following steps:
1. Data Cleaning
2. Feature Engineering
3. Model Training and Testing
4. Model Serialization (Pickling)
5. Deployment on AWS Elastic Beanstalk



# Forest Fires Prediction

A Flask-based web application that predicts wildfire risk in Algeria using environmental and meteorological factors. The model is trained on the Algerian Forest Fires dataset and deployed on AWS Elastic Beanstalk.

**Live Demo:** http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/

---

## Table of Contents

- [Project Overview](#project-overview)  
- [Repository Structure](#repository-structure)  
- [Dataset](#dataset)  
- [Modeling](#modeling)  
- [Web Application](#web-application)  
- [Installation & Usage](#installation--usage)  
- [Deployment](#deployment)  
- [Future Work](#future-work)  
- [License](#license)  

---

## Project Overview

Wildfires are influenced by many factors such as temperature, humidity, wind speed, and rainfall. This project:

1. Cleans and preprocesses the Algerian Forest Fires dataset.  
2. Engineers relevant features (e.g., seasonal encodings, fire weather indices).  
3. Trains regression models (Linear Regression, Random Forest, Gradient Boosting) to predict burned area.  
4. Serializes (pickles) the best-performing model.  
5. Serves predictions via a Flask app deployed on AWS Elastic Beanstalk.

---

## Repository Structure

ForestFiresPrediction/
├── .ebextensions/ # AWS Elastic Beanstalk configuration
├── models/ # Serialized (pickled) model files
├── notebooks/ # Jupyter notebooks for exploration & modeling
│ └── ALL_Regression.ipynb # End-to-end regression modeling pipeline
├── templates/ # Flask HTML templates
├── Algerian_forest_fires_dataset_UPDATE.csv
├── application.py # Flask application entry point
├── requirements.txt # Python dependencies
├── README.md # Project documentation
└── LICENSE # MIT License



---

## Dataset

- **Source:** Algerian Forest Fires Dataset  
- **File:** `Algerian_forest_fires_dataset_UPDATE.csv`  
- **Key Features:**  
  - `temp`, `RH` (relative humidity), `wind`, `rain`  
  - `FFMC`, `DMC`, `DC`, `ISI` (fire weather indices)  
  - `month`, `day` (encoded as categorical)  
- **Target:** `area` (burned area in hectares)

---

## Modeling

- **EDA & Cleaning** in `notebooks/ALL_Regression.ipynb`.  
- **Feature Engineering:**  
  - Encode `month`/`day` as numeric or one-hot  
  - Scale continuous variables  
- **Algorithms Tried:**  
  - Linear Regression  
  - Random Forest Regressor  
  - Gradient Boosting Regressor  
- **Evaluation:**  
  - K-fold cross-validation  
  - Mean Squared Error (MSE)  
- **Best Model:** Saved under `models/` (e.g. `best_model.pkl`)

---

## Web Application

- **Framework:** Flask  
- **Endpoints:**  
  - `/` – Input form for environmental parameters  
  - `/predict` – Returns predicted burned area  
- **Templates:**  
  - `templates/index.html` – Input form  
  - `templates/result.html` – Displays prediction  

---

## Installation & Usage

1. **Clone the repo**  
   ```bash
   git clone https://github.com/akhilesh360/ForestFiresPrediction.git
   cd ForestFiresPrediction
Create & activate a virtual environment

bash
Copy
Edit
python3 -m venv venv
source venv/bin/activate
Install dependencies

bash
Copy
Edit
pip install -r requirements.txt
Run the Flask app locally

bash
Copy
Edit
python application.py
Then open http://127.0.0.1:5000/ in your browser.

Deployment
AWS Elastic Beanstalk config is in .ebextensions/. To deploy:

Install & configure the EB CLI.

Initialize and create environment:

bash
Copy
Edit
eb init -p python-3.7 forestfiresprediction
eb create forestfiresprediction-env
Deploy updates:

bash
Copy
Edit
eb deploy
Live app: http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/

Future Work
Modeling: Try XGBoost or LightGBM; add spatial features (elevation, vegetation type)

Features: Incorporate lagged weather variables; satellite-derived indices

UX: Interactive risk maps; input validation

