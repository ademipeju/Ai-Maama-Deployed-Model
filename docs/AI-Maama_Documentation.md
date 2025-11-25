**AI-Maama: Postpartum Hemorrhage (PPH) or Severe Bleeding After Child Birth Risk Prediction System**



AI-powered maternal health monitoring for early detection and timely intervention



**1. Introduction**



AI-Maama is an AI-powered maternal health web application designed to support pregnant women and community healthcare workers by providing early, accurate prediction of Postpartum Hemorrhage (PPH) risks.



The platform uses machine learning to analyze vital signs and obstetric information, offering risk-level predictions classified as Low, Medium, or High.



The system consists of:



React Frontend — user-friendly interface for entering maternal vitals



FastAPI Backend — handles requests and returns predictions



XGBoost ML Model — the final predictive model trained on 500+ maternal records



AI-Maama empowers frontline health workers with data-driven decision support for early PPH risk detection.



**2. System Design**



AI-Maama follows a simple, accessible design that ensures usability for both pregnant women and community healthcare workers.



**Key Design Features**



Clean, intuitive UI for entering vital signs



Responsive React interface suitable for mobile or desktop



Predictive results displayed instantly



Color-coded risk levels (green = low, yellow = medium, red = high)



Clear prompts ensuring correct data entry



FastAPI backend communicates seamlessly with the AI model





**User Flow**



User logs into the web app



Inputs maternal vital signs into the form



The system sends the data to the FastAPI prediction endpoint



The backend loads the trained XGBoost model



Model generates PPH risk classification



Risk level is returned to the frontend and displayed





 **3. System Architecture**



AI-Maama uses a client–server architecture:



**A. Frontend (React)**



Collects user input



Sends POST request to backend API



Displays prediction result



Ensures smooth user experience



**B.** **Backend (FastAPI)**



Receives JSON input from frontend



Validates input



Loads preprocessing objects:



StandardScaler



Encoders (ordinal encoding)



Loads the trained ML model (XGBoost)



Generates prediction



Sends JSON response to frontend



**C. Machine Learning Layer**



Data preprocessing (scaling, mapping, encoding)



Feature selection



Model training and evaluation



Saving trained artifacts using joblib (.pkl files)



**D. Storage**



Local storage of:



model.pkl



scaler.pkl



feature\_columns.pkl



**E. Deployment**



FastAPI app deployed on a backend server



React app deployed separately





 **4. AI Models Used**



Two models were explored:



**Base Model: Random Forest**



Used as initial baseline model



Helped establish feature importance and general trends



**Final Model: XGBoost Classifier**



Chosen for its superior precision, accuracy, and ability to handle tabular data



Tuned using hyperparameter optimization



Achieved best performance score during cross-validation



XGBoost was ultimately deployed due to its:



High interpretability



Robustness to noisy medical data



Excellent performance on small-to-medium datasets





**5. Data Preprocessing**



To improve model accuracy and ensure reliable predictions, several preprocessing techniques were applied:



✔Standard Scaling



Used StandardScaler



Normalizes features such as:



Age



Blood pressure



Heart rate



Body temperature



BMI



✔ Feature Mapping



Custom mappings implemented for certain categorical features



✔ Ordinal Encoding



Applied to convert categories into numeric format the model can understand.



✔ Outlier Handling



Outliers were identified



Extreme values were capped (winsorization) to prevent skewed predictions



✔ Final Feature Set Used

\['Age', 'SystolicBP', 'DiastolicBP', 'BS', 'BodyTemp', 'HeartRate',

 'BMI', 'Anaemia', 'RiskLevel', 'Mode\_of\_delivery',

 'History\_of\_Past\_PPH', 'Parity']





**6. Dataset Details**



The dataset used to develop AI-Maama was sourced from:



Kaggle (public maternal health dataset)



Locally collected data to enrich the original dataset



Dataset Summary



Rows: 500



Columns: 12 features



Features include:



Age



Blood pressure (SBP, DBP)



Blood sugar



Body temperature



Heart rate



BMI



Anaemia



Parity



Mode of delivery



Past PPH history



Target: PPH Risk Level





**7. API Documentation (FastAPI)**

Endpoint

POST /predict



Input Format (JSON)



Example:



{

  "Age": 28,

  "SystolicBP": 120,

  "DiastolicBP": 80,

  "BS": 4.5,

  "BodyTemp": 37.2,

  "HeartRate": 85,

  "BMI": 26.5,

  "Anaemia": 0,

  "RiskLevel": 1,

  "Mode\_of\_delivery": 2,

  "History\_of\_Past\_PPH": 0,

  "Parity": 2

}



Output Format

{

  "risk\_prediction": "High"

}



 

**Backend Process**



Validate request



Transform input using scaler \& encoders



Reorder features based on feature\_columns.pkl



Model predicts probability \& class



Return risk category as response





**8. Model Inference Logic**



Risk categories defined as:



Low Risk



Medium Risk



High Risk



XGBoost outputs numerical class → mapped to human-readable labels.



Logic example:



risk\_map = {0: "Low", 1: "Medium", 2: "High"}





**9. Model Evaluation**



Train–test split



Cross-validation (k-fold)



Accuracy, F1-score



Confusion matrix



Hyperparameter tuning



Final model demonstrated:



Improved accuracy over baseline Random Forest



Greater stability across folds





**10. Frontend (React UI)**



The React interface:



Collects user vitals



Sends data to /predict endpoint via Axios or Fetch



Displays results clearly



Implements basic validation to prevent missing values





UI Goals:



Simplicity



Accessibility



Fast input



Mobile-friendly



**11. Saved Artifacts**



The following were exported using joblib and loaded during prediction:



XGBoost Model: my\_model.pkl



Scaler: scaler.pkl



Feature Columns: feature\_columns.pkl



These ensure identical preprocessing during training and prediction.





**Future Improvements**



Add SMS or email alerts for high-risk predictions



Build a maternal health dashboard for healthcare workers



Expand dataset with more records



Include additional risk indicators



Improve offline support for low-resource areas



**12. Conclusion**



AI-Maama provides a simple and accessible way to estimate the risk of severe postpartum bleeding.

It combines machine learning, a FastAPI backend, and a React frontend to deliver quick predictions that can help save lives and improve maternal care.

