# "Wings R Us": A Segmented Recommendation Engine

## Project Overview

This project is a solution for the **WWT Unravel 2025** competition. The goal is to design and build a recommendation algorithm for "Wings R Us," a US-based quick-service restaurant (QSR). The system provides smart, last-minute item suggestions to customers during the digital checkout process to increase average order value (AOV), enhance user satisfaction, and improve customer retention.

The solution is developed in a Python Jupyter Notebook (`WWT.ipynb`) and employs a sophisticated, segmented Market Basket Analysis using the Apriori algorithm to deliver personalized recommendations.

## The Business Problem

The leadership team at Wings R Us is looking to enhance their digital checkout experience by introducing smart item recommendations. They have plenty of data but need a clear strategy to drive these recommendations effectively.

Key questions from the client include:
> * Which customer signals or behaviors should drive recommendations?
> * How can the solution personalize suggestions for a diverse customer base (first-time users vs. loyal fans)?
> * How can recommendations remain fresh and non-repetitive without manual effort?
> * How should success be defined and measured?
> * How can the solution work seamlessly across the app, website, and kiosks?
> * What would a fast, value-proving pilot program look like?

## Data Sources

The analysis is based on four primary datasets provided for the competition:
* `order_data.csv`: Contains 1.4 million transactional records, including a nested JSON column with detailed order specifics.
* `customer_data.csv`: Contains information on 550,000 customers, primarily their customer type (e.g., Registered, Guest).
* `store_data.csv`: Contains location information for 38 stores.
* `test_data_question.csv`: The evaluation dataset with 1,000 customer cart configurations with one item removed, used for scoring.

## Our Approach & Methodology

Our solution follows a multi-phase approach, from data preparation to a highly segmented and validated modeling strategy.

### Phase 1: Data Preparation & Cleaning
The initial phase focused on creating a single, clean, and analysis-ready dataset.
* **JSON Parsing:** A custom function was built to parse the complex `ORDERS` JSON data into a structured, flat format.
* **Data Consolidation:** The three primary datasets (orders, customers, stores) were merged into a single master DataFrame.
* **Data Cleaning:** We performed several cleaning steps, including correcting data types (e.g., dates), standardizing all text to lowercase for consistency, handling missing values, and verifying the absence of duplicate records.

### Phase 2: Feature Engineering
To unlock deeper insights, we engineered several new features to model customer behavior and context:
* **Monetary Features:** `total_order_value`, `average_order_value`, `customer_total_spend`.
* **Behavioral Features:** `customer_frequency`, `order_size`, `recency_days`.
* **Contextual Features:** `order_day_of_week`, `order_hour`.

### Phase 3: Modeling with Market Basket Analysis
The core of our recommendation logic is built on the Apriori algorithm.
* **Transaction Formatting:** Each order was converted into a "transaction list" of items.
* **Association Rule Generation:** We used the `mlxtend` library to run the Apriori algorithm, generating hundreds of high-confidence "if-then" association rules. These rules were ranked by **Lift** and **Confidence** to identify the most statistically significant pairings.

### Phase 4: Advanced Personalization via Segmentation
Recognizing that one model does not fit all, we developed multiple specialized models.
* **Segmentation:** The dataset was segmented by **Customer Type** ('Registered', 'Guest', 'eClub') and further by **Order Occasion** ('ToGo', 'Delivery') to create highly specific recommendation contexts.
* **Specialized Models:** A unique set of association rules was trained for each of these segments, ensuring recommendations are tailored to the specific user and their context.

### Phase 5: Validation
The performance of each model was rigorously tested using a methodology that mirrors the competition's evaluation criteria.
* **Train-Test Split:** Data for each segment was split 70/30 for training and testing.
* **Metric:** We used **Recall@3** to measure performance, simulating how often our model could correctly predict a "missing" item from a customer's cart within its top three suggestions.

## Technical Requirements & Installation

This project was developed using Python 3.11. To set up the environment and install all necessary dependencies, run the following command in your terminal:

```bash
pip install -r requirements.txt
```

## How to Run the Code

1.  **Clone the Repository:**
    ```bash
    git clone <your-repository-url>
    ```
2.  **Install Dependencies:** Run the `pip install` command as shown above.
3.  **Place Data:** Ensure all the provided CSV files (`order_data.csv`, `customer_data.csv`, etc.) are in the same root directory as the Jupyter Notebook.
4.  **Execute Notebook:** Open and run the `WWT.ipynb` notebook from the first cell to the last. The notebook is designed to be run sequentially and will generate several intermediate and final CSV files in the process.

## Key Findings & Results

* **EDA Insights:** Our exploratory analysis revealed that **Registered users make up 72%** of the customer base, and **ToGo orders account for 86%** of all transactions, highlighting the most critical segments to optimize.
* **Model Performance:** Our segmented modeling approach proved effective, achieving a **Recall@3 score of ~23%** for our best-performing segment. The variation in scores between segments confirms that the personalized strategy is more effective than a single, generalized model.

## Future Scope & Improvements

* **Real-Time Inventory Check:** Integrate the recommendation engine with a store's real-time inventory system to prevent recommending out-of-stock items.
* **Advanced Modeling:** Experiment with more advanced recommendation algorithms like collaborative filtering or deep learning models to capture more nuanced user preferences.
* **Automated Retraining Pipeline:** Develop an MLOps pipeline to automatically retrain and deploy the models on a regular schedule, ensuring the recommendations stay fresh and adapt to new trends.
