# Netflix Prize Recommendation System

## Project Overview:

This project builds and evaluates movie recommendation systems using a subset of the Netflix Prize dataset.

The objective was to compare different recommendation approaches and analyze their performance using both prediction-based and ranking-based evaluation metrics.

This project includes:

- Data preprocessing and cleaning
- Exploratory Data Analysis (EDA)
- Time-based train/test splitting
- Item-Based Collaborative Filtering
- SVD Matrix Factorization 
- Recommendation quality evaluation
- Recommendation analysis and failure-case investigation

---

## Dataset:

### Source

Netflix Prize Dataset

### Dataset Used

A subset of the original Netflix Prize dataset was used for experimentation.

Initial dataset:

- 5000000 ratings
- 404478 users
- 996 movies

After filtering users with fewer than 40 ratings:

- 1516820 ratings
- 25023 users
- 996 movies

### Sparsity

The resulting user-item matrix had a sparsity of:

**98.43%**

This highlights the challenge of recommendation systems, where only a very small fraction of all possible user-movie interactions are observed.

---

## Data Processing Pipeline:

The Netflix dataset uses a custom format where each movie is followed by its ratings:

```text
1:
1488844,3,2005-09-06
822109,5,2005-05-13
...
```

The preprocessing pipeline:

1. Parses movie headers and rating records
2. Converts data into tabular format
3. Processes movie metadata
4. Converts date fields into date-time format
5. Filters low activity users
6. Creates the final modeling dataset

Notebook:

```text
notebooks/data_processing_pipeline.ipynb
```

---

## Exploratory Data Analysis:

The following analyses were performed:

### Rating Distribution

- Ratings are concentrated around 4 and 5.
- Users tend to rate movies they voluntarily choose to watch.
- Positive ratings are significantly more common than negative ratings.

### User Activity

- User activity follows a long-tail distribution.
- Most users rate only a small number of movies.
- A small group of highly active users contributes a large portion of the ratings.

### Movie Popularity

- Ratings are heavily concentrated among a small set of popular movies.
- The majority of movies receive relatively few ratings.
- Several low-popularity movies exhibit very high average ratings due to small sample sizes.

### Temporal Analysis

- Rating volume increases significantly over time.
- Average ratings remain relatively stable despite growth in activity.

---

## Modeling Approach:

A time-based train/test split was used.

For each user:

- Oldest 80% of ratings goes to Training Set
- Newest 20% of ratings goes to Test Set

This reflects real-world recommendation scenarios where future interactions must be predicted from past behavior.

---

## Models Implemented:

### 1. Baseline Predictor

The baseline model predicts ratings using:

- Global average rating
- User bias
- Item bias

This serves as a simple benchmark.

---

### 2. Item-Based Collaborative Filtering

Similarity Metric:

- Cosine Similarity

Characteristics:

- Recommends movies based on similar items
- Uses item-item relationships
- More scalable than user-based collaborative filtering

---

### 3. Singular Value Decomposition (SVD)

Matrix factorization was used to learn latent user and movie representations.

Parameters:

```python
n_factors = 100
n_epochs = 20
random_state = 42
```

Advantages:

- Handles sparse datasets effectively
- Learns hidden preference patterns
- Produces more stable predictions

---

## Evaluation Metrics:

We used Multiple evaluation metrics.

### Prediction Metrics

- RMSE
- MAE

### Ranking Metrics

- MAP@10
- Precision@10
- Recall@10
- Coverage

---

## Key Findings:

### RMSE Performance

SVD achieved the best prediction performance and performed better than Item-Based Collaborative Filtering.

Key observations:

- Both Item-CF and SVD outperformed the baseline model.
- SVD produced more stable predictions.
- Item-CF occasionally generated larger prediction errors.

### Ranking Performance

An interesting observation was that better RMSE did not automatically means a better ranking
performance.

The experiments showed:

- Some low-popularity movies received very high predicted scores.
- These movies frequently appeared in recommendation lists.
- Ranking metrics were therefore affected differently from prediction metrics.

This highlights the difference between:

- Rating prediction quality
- Recommendation ranking quality

---

## Popularity Filtering Experiment:

To investigate ranking performance, we applied popularity-based filtering.

Movies below a minimum popularity threshold were excluded from recommendations.

Results showed:

- Recommendation quality improved as very low-popularity movies were removed.
- Best performance occurred around:

```text
min_pop = 8000
```

with:

```text
MAP@10 = 0.0669
```

Increasing the threshold further reduced recommendation diversity and lowered performance.

This demonstrated a trade-off between:

- Recommendation accuracy
- Catalog coverage

---

## Recommendation Analysis:

Qualitative recommendation analysis revealed several interesting patterns.

### Successful Cases

- The model successfully recommended niche content such as *Inu-Yasha: The Movie 3*.
- Personalized recommendations were generated for highly active users.
- User-specific rating behavior was captured effectively.

### Failure Cases

- TV seasons were recommended without understanding viewing order.
- Special editions and bonus materials were sometimes over-recommended.
- Highly active users received increasingly niche recommendations.
- Recommendation diversity was occasionally limited.

---

## Repository Structure:

## Repository Structure

```text
netflix-recommendation-system/
│
├── notebooks/
│   ├── data_processing_pipeline.ipynb
│   └── model_training_pipeline.ipynb
│
├── results/
│   ├── evaluation_metrics.txt
│   └── recommendation_examples.csv
│
├── Presentation_Netflix.pdf
├── Technical_Report_Netflix.pdf
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## How to Reproduce Results:

### 1. Clone Repository

```bash
git clone https://github.com/harshdeep-1234/netflix-recommendation-system.git
cd netflix-recommendation-system
```

### 2. Create Virtual Environment

```bash
python -m venv venv
```

Activate:

Windows:

```bash
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Notebooks

Execute in the following order:

1. `data_processing_pipeline.ipynb`
2. `model_training_pipeline.ipynb`

---

## Authors

Harsh Deep
&
Anuprash Pattnaik

Team Project – Netflix Recommendation System