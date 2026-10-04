# Complex-Systems-and-Applications-Final-Project
# Hospital Infection Control: Can Earlier Transfers Help Predict Later Clusters?

## 1. Project Description
This project explores whether tracking patient transfers across a hospital network can improve our ability to predict future synthetic infection clusters[cite: 8]. 

## 2. Problem Statement
The central objective was to predict which previously unaffected hospital wards would develop a new infection cluster during a 14-day follow-up window[cite: 4, 8]. Specifically, I wanted to see if directed patient transfers add predictive information beyond a ward's basic attributes[cite: 8].

## 3. Data Set
The analysis uses a synthetic teaching dataset representing a regional hospital group[cite: 2, 8]. It consists of two files:
* `hospital_wards.csv`: Contains 120 wards (nodes) with baseline features like `occupancy_ratio` and `device_use_share`[cite: 2, 8]. 18 wards had an active cluster at the start, leaving 102 eligible for prediction[cite: 2].
* `hospital_transfers.csv`: Contains 420 directed transfer links representing patient movement and volume in the 14 days prior to the prediction point[cite: 2, 8].

## 4. Method
I constructed a directed network graph using wards as nodes and the transfer counts as weighted edges[cite: 8]. Using this graph, I engineered two new features using only information available at the observation point[cite: 2]: 
1. `incoming_volume`: Total patients transferred into the ward[cite: 4].
2. `from_active_volume`: Patients transferred directly from wards that already had an active cluster[cite: 4].

I then set aside 30% of the eligible wards for testing and compared two models[cite: 4]. Model A used only the original ward features, while Model B included the two new network features. I used a logistic regression pipeline with a standard scaler fitted only on the training data[cite: 4].

## 5. Results
Adding the network features noticeably improved predictive performance on the test set. The logistic regression model with the network features achieved higher F1 and ROC AUC scores than the baseline model[cite: 4]. Most notably, looking at the confusion matrices, the baseline model had a frustratingly high number of false negatives (missed clusters), which the network data helped reduce[cite: 4].

## 6. Interpretation
The network features definitely help. The two new metrics capture different risks: total incoming volume tracks general ward turnover, while transfers from affected wards act as a direct proxy for known exposure[cite: 2]. Edge direction is crucial because incoming transfers bring potential pathogens into a ward, whereas outgoing transfers do not[cite: 2]. 

Ethically, relying solely on this model to manage a real hospital would be dangerous. While a false alarm wastes precious infection-control resources, a missed cluster (false negative) is much worse, potentially exposing vulnerable patients and staff to an outbreak[cite: 4]. These predictions should only serve as an early warning system that human clinical teams review before taking action[cite: 4, 8].

## 7. Reflection
**What worked well:** Engineering graph-based features and plugging them directly into a standard scikit-learn pipeline was seamless and clearly showed the value of relational data. 
**What was difficult:** The baseline features (`occupancy_ratio` and `device_use_share`) had massive overlap between the target classes, making the initial EDA a bit discouraging before the network data was introduced. 
**What could be improved:** With more time, I would explore alternative classification models, like Random Forests or Gradient Boosting, to see if they capture non-linear relationships better than logistic regression.
