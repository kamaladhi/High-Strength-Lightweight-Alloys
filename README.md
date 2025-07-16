# Project Title: Design of Alloys for gaining high strength to weight ratio
## **Overview:**

This repository contains machine learning models developed for the "Design of Alloys with High Strength-to-Weight Ratio" project . The models aim to predict key properties of Aluminum (Al) alloys, specifically minimum density and maximum stress, to facilitate the design of new alloys with improved strength-to-weight ratios.

## **Features:**

- Property Prediction: Machine learning models (Support Vector Regression - SVR) for predicting maximum stress and minimum density of Al alloys.

- Composition-Based Featurization: Utilizes the CBFV (Composition-Based Feature Vectors) library to generate relevant features from alloy chemical formulas.

- Data Preprocessing: Includes steps for data splitting (90% train, 10% test), feature scaling using StandardScaler, and handling of elemental properties.

- Robust Model Training: Employs GridSearchCV with GroupKFold cross-validation for hyperparameter tuning and robust model selection. GroupKFold ensures that samples with the same alloy composition are kept together during cross-validation, preventing data leakage.

- Uncertainty Quantification: Implements a bootstrapping method to estimate the mean and standard deviation of predictions, providing a measure of uncertainty for each prediction.

- Expected Improvement Calculation: Includes functionality to calculate Expected Improvement (EI), an acquisition function used in Bayesian optimization to identify promising new alloy compositions for further investigation.


## Repository Structure
* `Density_prediction_model.ipynb`: Jupyter Notebook containing the code for predicting minimum density of Al alloys.
* `maxStress_prediction_model.ipynb`: Jupyter Notebook containing the code for predicting maximum stress of Al alloys.
* `Density_target.xlsx`: Excel file containing the target density data for training and testing the density prediction model.
* `maxStress_target.xlsx`: Excel file containing the target maximum stress data for training and testing the stress prediction model.
* `README.md`: This file.

## Getting Started

### Prerequisites

* Python 3.x
* Jupyter Notebook

### Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/kamaladhi/High-Strength-Lightweight-Alloys.git](https://github.com/kamaladhi/High-Strength-Lightweight-Alloys.git)
    cd High-Strength-Lightweight-Alloys
    ```

2.  **Install the required libraries:**
    The notebooks use CBFV for featurization, along with standard libraries like pandas, numpy, scikit-learn, and matplotlib.
    ```bash
    pip install pandas numpy scikit-learn matplotlib seaborn tqdm scipy
    pip install CBFV
    ```

### Usage

1.  **Open the Jupyter Notebooks:**
    ```bash
    jupyter notebook
    ```
2.  Navigate to and open `Density_prediction_model.ipynb` or `maxStress_prediction_model.ipynb`.
3.  **Run all cells** in the notebooks sequentially.

    * The notebooks will:
        * Load the respective `_target.xlsx` files for training and testing.
        * Generate features from the alloy formulas.
        * Train and evaluate the SVR models.
        * Perform bootstrapping for uncertainty quantification.
        * Load `Al-Virtual-Samples.xlsx` to predict properties for the candidate alloys.
        * Calculate Expected Improvement (EI) for new design candidates.
        * Save the prediction results (including EI) to a CSV file (e.g., `Denisty_prediction.csv` or `max_stress_pred.csv`).

## Models and Metrics

### Maximum Stress Prediction Model

* **Model**: Support Vector Regression (SVR)
* **Training R-squared**: ~0.729
* **Test R-squared**: ~0.668
* **Mean Absolute Error (MAE) (Test)**: ~488.77 MPa
* **Root Mean Squared Error (RMSE) (Test)**: ~597.99 MPa

### Minimum Density Prediction Model

* **Model**: Support Vector Regression (SVR)
* **Training R-squared**: ~0.947
* **Test R-squared**: ~0.910
* **Mean Absolute Error (MAE) (Test)**: ~0.323 g/cm³
* **Root Mean Squared Error (RMSE) (Test)**: ~0.519 g/cm³

(Note: For minimum density, the Expected Improvement calculation in the notebook currently identifies "improvement" as values higher than the current maximum observed density. For true minimization, this acquisition function would ideally be adapted to find compositions that minimize the density, or a Lower Confidence Bound (LCB) acquisition function could be used.)

## Contributing

Contributions are welcome! If you have suggestions for improving the models, adding new features, or fixing bugs, please feel free to open an issue or submit a pull request.

## License

*(You might want to add a license here, e.g., MIT, Apache 2.0, etc.)*
