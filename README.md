# Hotel Customer Segmentation

A clustering project that segments hotel customers into distinct behavioural groups to support retention, re-engagement, loyalty, and channel strategy.

The analysis follows the CRISP-DM methodology and uses customer recency, revenue, and relationship tenure to create a data-driven segmentation for a hotel in Lisbon.

## Business Objective

The hotel’s existing segmentation was based mainly on booking origin and did not capture meaningful differences in customer behaviour.

This project aims to identify customer groups that can support:

- customer retention;
- re-engagement campaigns;
- loyalty initiatives;
- channel prioritisation;
- differentiated marketing actions.

## Dataset

The project uses `HotelCustomersDataset.tsv`.

The original dataset contains:

- 83,590 customer records;
- 31 variables;
- customer demographics;
- booking activity;
- revenue;
- stay recency;
- distribution channel;
- market segment;
- special requests.

Dataset reference:

Nuno António, 2022, Revision 1.0.

## Data Preparation

The main preparation steps included:

- removing 19,920 inactive records with no stay or revenue activity;
- removing exact duplicate records;
- merging fragmented customer profiles using `NameHash` and `DocIDHash`;
- removing implausible ages and customers under 18;
- imputing missing age values;
- capping extreme revenue and lead-time values;
- engineering `TotalRevenue` and `Tenure`;
- retaining categorical variables for cluster profiling;
- scaling the modelling variables with `MinMaxScaler`.

After preparation, the final dataset contained **60,903 active adult customers**.

## Features Used for Clustering

An exhaustive feature search identified the following variables as the strongest combination:

- `DaysSinceLastStay`
- `TotalRevenue`
- `Tenure`

Other variables were retained for post-clustering interpretation.

## Modeling

Hierarchical clustering was first applied to a random sample of 2,000 customers as an exploratory step.

K-Means was then applied to the full prepared dataset.

Values of K from 3 to 7 were compared using:

- inertia;
- silhouette score;
- Davies-Bouldin score;
- Calinski-Harabasz score;
- cluster-size balance;
- business interpretability.

## Selected Solution

A **K-Means model with K = 4** was selected.

Model results:

- Silhouette score: **0.462**
- Davies-Bouldin score: **0.774**
- Calinski-Harabasz score: **70,254.78**

K = 4 was selected because it provided strong cluster separation and produced four distinct, actionable customer groups.

## Customer Segments

| Segment | Customers | Share | Main Profile |
|---|---:|---:|---|
| Lapsed Guests | 19,638 | 32.2% | Customers who stayed one to two years ago and may respond to re-engagement campaigns |
| Active Guests | 18,984 | 31.2% | Recently active customers and the main short-term retention target |
| Dormant Guests | 18,664 | 30.6% | Long-inactive customers suited to low-cost reactivation campaigns |
| High-Value Loyal Guests | 3,617 | 5.9% | High-revenue, longer-tenure customers suited to loyalty and premium treatment |

### High-Value Loyal Guests

This segment generated average revenue of approximately **€1,588**, nearly four times the revenue of the other groups. These customers also showed a stronger preference for direct bookings.

### Active Guests

These customers had the most recent stays and represent the strongest opportunity for retention and loyalty-programme enrolment.

### Lapsed Guests

These customers had not returned for one to two years and represent the main audience for targeted win-back campaigns.

### Dormant Guests

These customers had been inactive for the longest period and are more suitable for lower-cost, broad reactivation campaigns.

## Main Findings

- 23.8% of the original records had no stay or revenue activity.
- 4,093 records represented fragmented customer histories that required consolidation.
- The hotel’s existing origin-based segmentation provided limited differentiation between most behavioural groups.
- Portuguese customers were more represented in the Dormant segment and less represented in the High-Value Loyal segment.
- The engineered `Tenure` variable helped identify the commercially important High-Value Loyal group.

## Repository Structure

```text
hotel-customer-segmentation/
│
├── data/
│   ├── HotelCustomersDataset.tsv
│   └── processed/
│       ├── X_model.csv
│       ├── X_unscaled.csv
│       ├── df_profile.csv
│       ├── minmax_scaler.pkl
│       └── model_features.pkl
│
├── notebooks/
│   ├── 01_business_data_understanding.ipynb
│   ├── 02_data_preparation.ipynb
│   ├── 03_modeling.ipynb
│   └── 04_evaluation_deployment.ipynb
│
├── .gitignore
├── README.md
└── requirements.txt
```

## Running the Project

Clone the repository:

```bash
git clone <repository-url>
cd hotel-customer-segmentation
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

Start JupyterLab:

```bash
jupyter lab
```

Run the notebooks in this order:

1. `01_business_data_understanding.ipynb`
2. `02_data_preparation.ipynb`
3. `03_modeling.ipynb`
4. `04_evaluation_deployment.ipynb`

## Technologies

- Python
- pandas
- NumPy
- Matplotlib
- Seaborn
- scikit-learn
- SciPy
- joblib
- JupyterLab

## Limitations

- Hierarchical clustering was performed on one 2,000-customer sample.
- The three largest segments are separated mainly by recency.
- Most customers have only one recorded checked-in booking.
- Transaction-level data could provide stronger behavioural differentiation.
- The deployment section presents a proposed implementation rather than a production system.

## Future Improvements

- validate the segmentation on newer customer data;
- test multiple hierarchical-clustering samples;
- incorporate transaction-level and campaign-response data;
- monitor segment size and profile changes over time;
- export the final segmentation table for CRM use;
- save the fitted K-Means model for scoring new customers.

## License

Add a licence file before publishing if you want to define how others may reuse the project.
