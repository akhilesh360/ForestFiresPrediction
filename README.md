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



```markdown
# Forest Fires Prediction

A Flask-based web application that predicts wildfire risk in Algeria using meteorological and environmental factors. The model is trained on the Algerian Forest Fires dataset and deployed on AWS Elastic Beanstalk.

**Live Demo:** http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Repository Structure](#repository-structure)  
3. [Dataset](#dataset)  
4. [Modeling](#modeling)  
5. [Web Application](#web-application)  
6. [Installation & Usage](#installation--usage)  
7. [Deployment](#deployment)  
8. [Future Work](#future-work)  
9. [License](#license)  

---

## Project Overview

Wildfires depend on factors such as temperature, humidity, wind speed, and rainfall. This project:

- Cleans and preprocesses the Algerian Forest Fires dataset.  
- Engineers features (e.g., seasonal encodings, fire weather indices).  
- Trains regression models (Linear Regression, Random Forest, Gradient Boosting) to predict burned area.  
- Serializes (pickles) the best-performing model.  
- Serves predictions via a Flask app deployed on AWS Elastic Beanstalk.

---

## Repository Structure

```

ForestFiresPrediction/
├── .ebextensions/                # AWS Elastic Beanstalk configuration
├── models/                       # Serialized (pickled) model files
├── notebooks/                    # Jupyter notebooks for exploration & modeling
│   └── ALL\_Regression.ipynb      # End-to-end regression pipeline
├── templates/                    # Flask HTML templates
│   ├── index.html                # Input form
│   └── result.html               # Prediction display
├── Algerian\_forest\_fires\_dataset\_UPDATE.csv
├── application.py                # Flask entry point
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation
└── LICENSE                       # MIT License

````

---

## Dataset

- **Source:** Algerian Forest Fires Dataset  
- **File:** `Algerian_forest_fires_dataset_UPDATE.csv`  
- **Features:**  
  - **Meteorological:** `temp`, `RH` (relative humidity), `wind`, `rain`  
  - **Fire Weather Indices:** `FFMC`, `DMC`, `DC`, `ISI`  
  - **Temporal:** `month`, `day` (categorical)  
- **Target:** `area` (burned area in hectares)

---

## Modeling

- **Exploration & Cleaning:** Performed in `notebooks/ALL_Regression.ipynb`.  
- **Feature Engineering:**  
  - Encode `month`/`day` as numeric or one-hot vectors.  
  - Scale continuous variables for regression.  
- **Algorithms Evaluated:**  
  - Linear Regression  
  - Random Forest Regressor  
  - Gradient Boosting Regressor  
- **Evaluation Metrics:**  
  - K-fold cross-validation  
  - Mean Squared Error (MSE)  
- **Best Model:** Saved under `models/` (e.g., `best_model.pkl`).

---

## Web Application

- **Framework:** Flask  
- **Routes:**  
  - `GET /` — Renders input form.  
  - `POST /predict` — Returns predicted burned area.  
- **Templates:**  
  - `index.html` — Parameter input fields.  
  - `result.html` — Displays prediction and summary.

---

## Installation & Usage

1. **Clone the repository**  
   ```bash
   git clone https://github.com/akhilesh360/ForestFiresPrediction.git
   cd ForestFiresPrediction
````

2. **Create & activate a virtual environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```
3. **Install dependencies**

   ```bash
   pip install -r requirements.txt
   ```
4. **Run the Flask app locally**

   ```bash
   python application.py
   ```

   Open [http://127.0.0.1:5000/](http://127.0.0.1:5000/) in your browser.

---

## Deployment

Configuration for AWS Elastic Beanstalk is provided in `.ebextensions/`. To deploy:

1. **Install & configure the EB CLI**
2. **Initialize & create environment**

   ```bash
   eb init -p python-3.7 forestfiresprediction
   eb create forestfiresprediction-env
   ```
3. **Deploy updates**

   ```bash
   eb deploy
   ```

The live app is at:
[http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/](http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/)

---

## Future Work

* **Model Enhancements:** Try XGBoost or LightGBM; include spatial features (elevation, vegetation type).
* **Feature Expansion:** Add lagged weather variables; incorporate satellite-derived indices.
* **User Experience:** Interactive risk maps; form validation and error handling.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

```
```

