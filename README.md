# 🥗 Dietra Nutrition Prediction Model

This repository contains the trained **MultiOutputRegressor** model built using [CatBoost](https://catboost.ai/) for the **Dietra App**. The model predicts personalized daily nutrition requirements based on a user's health, activity, and physiological data.

### 🔮 Model Predictions:
- **Daily Calorie Target (kcal)**
- **Protein (g)**
- **Carbohydrates (g)**
- **Fat (g)**

---

## 📊 Input Features

The model takes the following user-specific and device-collected inputs:

| Feature                   | Type     | Description |
|--------------------------|----------|-------------|
| `age`                    | int      | Age in years |
| `gender`                 | int      | 0 = female, 1 = male |
| `height_cm`              | float    | Height in centimeters |
| `weight_kg`              | float    | Weight in kilograms |
| `bmi`                    | float    | Body Mass Index |
| `goal`                   | int      | Encoded health/fitness goal |
| `medical_conditions`     | int      | Encoded health condition(s) |
| `allergies`              | int      | Encoded food/environmental allergies |
| `menstrual_cycle_regular`| int      | 0 = none, 1 = no, 2 = yes |
| `cycle_length_days`      | int      | Average length of menstrual cycle |
| `avg_water_intake`       | float    | Daily average water intake in liters |
| `avg_sleep_hours`        | float    | Daily average sleep duration |
| `avg_daily_steps`        | int      | Average steps per day |
| `avg_heart_rate`         | int      | Average heart rate (bpm) |
| `avg_calorie_burned`     | float    | Average daily calories burned |
| `category`               | int      | 0 = athlete, 1 = normal |

📁 Refer to [`input_variables.json`](./input_variables.json) for schema and data types.

---

## 🧠 Label Encodings

Categorical variables are encoded using mappings defined in [`label_encoding.json`](./label_encoding.json).

### Examples:
- **Gender**: `{"female": 0, "male": 1}`
- **Goal**: `{"gain muscle": 0, ..., "reduce stress": 10}`
- **Medical Conditions**: `{"diabetes": 9, "none": 13, ...}`
- **Allergies**: `{"nuts": 3, "none": 2, ...}`
- **Menstrual Regularity**: `{"none": 0, "no": 1, "yes": 2}`
- **Category**: `{"athlete": 0, "normal": 1}`

---

## 🧪 Model Details

- **Algorithm**: `MultiOutputRegressor` wrapping `CatBoostRegressor`
- **Framework**: `scikit-learn`, `catboost`
- **Persistence Format**: Pickle (`.pkl`)
- **Target Outputs**:
  - `calorie_target`
  - `protein_g`
  - `carbs_g`
  - `fat_g`

---

## 💻 Usage Example

```python
import pickle
import pandas as pd

# Load the trained model
with open("nutrition_model.pkl", "rb") as f:
    model = pickle.load(f)

# Example input (must match schema and encoding)
sample_input = pd.DataFrame([{
    "age": 25,
    "gender": 1,
    "height_cm": 175.0,
    "weight_kg": 70.0,
    "bmi": 22.9,
    "goal": 4,
    "medical_conditions": 13,
    "allergies": 2,
    "menstrual_cycle_regular": 0,
    "cycle_length_days": 0,
    "avg_water_intake": 2.5,
    "avg_sleep_hours": 7.5,
    "avg_daily_steps": 8000,
    "avg_heart_rate": 72,
    "avg_calorie_burned": 2300.5,
    "category": 1
}])

# Predict nutrition needs
predicted_values = model.predict(sample_input)
print("Predicted [Calories, Protein(g), Carbs(g), Fat(g)]:", predicted_values)
