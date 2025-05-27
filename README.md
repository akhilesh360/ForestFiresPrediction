
# Forest Fires Prediction

A Flask-based web application that predicts wildfire risk in Algeria using meteorological and environmental factors. The model is trained on the Algerian Forest Fires dataset and deployed on AWS Elastic Beanstalk.

**Live Demo:** http://forestfiresprediction-env-1.eba-djbn9yjg.us-east-2.elasticbeanstalk.com/

---

<img width="1709" alt="Screenshot 2025-05-26 at 10 43 22 PM" src="https://github.com/user-attachments/assets/65590329-398e-460d-9875-b9234dbad514" />


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
- Serializes (`pickle`) the best-performing model.  
- Serves predictions via a Flask app deployed on AWS Elastic Beanstalk.

## Repository Structure

The project is organized as follows:

```text
ForestFiresPrediction/
├── .ebextensions/             # AWS Elastic Beanstalk settings (configuration files)
│   └── python.config          # Specifies WSGI path for deployment
├── models/                    # Pickled model artifacts
│   └── best_model.pkl         # Serialized model used by the application
├── notebooks/                 # Data exploration and model development
│   └── ALL_Regression.ipynb   # End-to-end regression workflow (EDA, training, evaluation)
├── templates/                 # Flask HTML templates for the user interface
│   ├── index.html             # Form for feature input
│   └── result.html            # Displays prediction results
├── Algerian_forest_fires_dataset_UPDATE.csv  # Raw dataset file
├── application.py             # Flask application entry point
├── requirements.txt           # Project dependencies
├── README.md                  # Project documentation (this file)
└── LICENSE                    # License information (MIT License)
````

## Dataset

* **Source:** Algerian Forest Fires Dataset
* **File:** `Algerian_forest_fires_dataset_UPDATE.csv`
* **Features:**

  * **Meteorological:** `temp`, `RH` (relative humidity), `wind`, `rain`
  * **Fire Weather Indices:** `FFMC`, `DMC`, `DC`, `ISI`
  * **Temporal:** `month`, `day` (categorical)
* **Target:** `area` (burned area in hectares)

## Modeling

```python
# Example snippet from notebooks/ALL_Regression.ipynb
import pandas as pd
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_squared_error

data = pd.read_csv('Algerian_forest_fires_dataset_UPDATE.csv')
# Preprocessing
data = pd.get_dummies(data, columns=['month','day'], drop_first=True)
X = data.drop('area', axis=1)
y = data['area']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Model training
model = RandomForestRegressor(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
print("MSE:", mean_squared_error(y_test, y_pred))

# Serialize best model
import pickle
with open('models/best_model.pkl', 'wb') as f:
    pickle.dump(model, f)
```

## Web Application

```python
# application.py
from flask import Flask, render_template, request
import pickle
import numpy as np

app = Flask(__name__)
model = pickle.load(open('models/best_model.pkl', 'rb'))

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/predict', methods=['POST'])
def predict():
    # Collect inputs from form
    int_features = [float(request.form.get(f)) for f in ['FFMC','DMC','DC','ISI','temp','RH','wind','rain']]
    # Add month/day dummies if needed
    final_features = np.array(int_features).reshape(1, -1)
    prediction = model.predict(final_features)[0]
    return render_template('result.html', prediction=round(prediction, 2))

if __name__ == "__main__":
    app.run(debug=True)
```

### HTML Templates

**templates/index.html**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Forest Fire Risk Prediction</title>
  </head>
  <body>
    <h1>Predict Burned Area (hectares)</h1>
    <form action="/predict" method="post">
      {% for feature in ['FFMC','DMC','DC','ISI','temp','RH','wind','rain'] %}
      <label>{{ feature }}:</label>
      <input type="text" name="{{ feature }}"><br>
      {% endfor %}
      <button type="submit">Predict</button>
    </form>
  </body>
</html>
```

**templates/result.html**

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Prediction Result</title>
  </head>
  <body>
    <h1>Predicted Burned Area: {{ prediction }} hectares</h1>
    <a href="/">Make another prediction</a>
  </body>
</html>
```

## Installation & Usage

```bash
# Clone the repository
git clone https://github.com/akhilesh360/ForestFiresPrediction.git
cd ForestFiresPrediction

# Create & activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run locally
python application.py
# Open http://127.0.0.1:5000/ in your browser
```

## requirements.txt

```text
Flask==2.2.2
numpy==1.23.5
pandas==1.5.3
scikit-learn==1.2.2
gunicorn==20.1.0
```

## Deployment

AWS Elastic Beanstalk configuration is in `.ebextensions/python.config`:

```yaml
option_settings:
  aws:elasticbeanstalk:container:python:
    WSGIPath: application.py
```

```bash
# Initialize & deploy
eb init -p python-3.7 forestfiresprediction
eb create forestfiresprediction-env
eb deploy
```

## Future Work

* Try XGBoost or LightGBM; include spatial features (elevation, vegetation type).
* Feature expansion: lagged weather variables; satellite-derived indices.
* UX: interactive risk maps; form validation.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

```

