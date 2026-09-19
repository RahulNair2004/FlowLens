# 🚦 FlowLens — Development Roadmap

## Project Goal

Build FlowLens from a research-oriented ML prototype into a production-grade urban mobility intelligence platform.

The development process is divided into sequential phases.

```text
Phase 1
Product Definition & System Design
        ↓
Phase 2
Data Acquisition & Data Engineering
        ↓
Phase 3
EDA & Feature Engineering
        ↓
Phase 4
ML Baselines
        ↓
Phase 5
Advanced ML & Uncertainty
        ↓
Phase 6
Risk & Reliability Engine
        ↓
Phase 7
Recommendation & What-If Engine
        ↓
Phase 8
Backend & Database
        ↓
Phase 9
Frontend & User Experience
        ↓
Phase 10
End-to-End Integration
        ↓
Phase 11
Explainability & AI Layer
        ↓
Phase 12
Testing & Evaluation
        ↓
Phase 13
Containerization & Deployment
        ↓
Phase 14
Productionization
        ↓
Phase 15
V2 / Advanced Mobility Intelligence
```

---

# Phase 1 — Product Definition & System Design

### Objective

Define exactly what FlowLens is, what problem it solves, what the MVP contains, what the ML system predicts, what data is required, and how the components will interact.

### Duration

2–3 days

### Tasks

* Define product vision
* Define problem statement
* Identify target users
* Define MVP
* Define non-goals
* Define V1/V2/V3 features
* Define ML problems
* Define data requirements
* Select initial geographic scope
* Design system architecture
* Design ML pipeline
* Design database conceptually
* Design API structure
* Design user journey
* Create repository structure
* Create documentation

### Deliverables

```text
docs/
├── product.md
├── roadmap.md
├── ml_design.md
├── data_strategy.md
├── architecture.md
├── database.md
└── api_design.md
```

Plus:

```text
Architecture diagram
ML pipeline diagram
Database diagram
User flow
Initial README
```

### Definition of Done

A developer should be able to understand the complete FlowLens concept without looking at source code.

### Do Not Build Yet

* ML models
* React application
* FastAPI implementation
* Docker
* AWS
* Real-time infrastructure
* Large-scale data downloads

---

# Phase 2 — Data Acquisition & Data Engineering

### Objective

Acquire and transform the datasets required for the first FlowLens ML pipeline.

### Initial geographic focus

New York City.

### Primary data categories

```text
Mobility
Transit
Weather
Calendar
Events / Context
```

### Candidate sources

```text
NYC TLC Trip Record Data
GTFS
GTFS-Realtime where applicable
Historical Weather
Public Holiday / Calendar Data
Event Data where reliable
```

### Tasks

1. Investigate available datasets.
2. Evaluate licensing and usage restrictions.
3. Select exact datasets.
4. Select time periods.
5. Download only required files.
6. Create raw data storage.
7. Inspect schemas.
8. Remove corrupted records.
9. Handle missing values.
10. Remove impossible trips.
11. Normalize timestamps.
12. Normalize geographic information.
13. Join contextual datasets.
14. Create reproducible preprocessing scripts.
15. Generate a unified analytical dataset.

### Expected structure

```text
data/
├── raw/
├── interim/
├── processed/
└── metadata/
```

### Deliverables

```text
Raw datasets
Cleaned datasets
Data dictionary
Data validation scripts
Preprocessing pipeline
Dataset documentation
```

### Definition of Done

A reproducible script can transform the selected raw datasets into the FlowLens training dataset.

---

# Phase 3 — EDA & Feature Engineering

### Objective

Understand the mobility data before training models.

### Questions

Investigate:

* How does travel time change by hour?
* How does it change by weekday?
* Which routes/areas have high variability?
* How does weather affect travel?
* What are the major outliers?
* Are there seasonal patterns?
* How much missing data exists?
* Which features are predictive?
* Are there data leakage risks?

### Feature groups

#### Temporal

```text
hour
day_of_week
month
week_of_year
is_weekend
is_holiday
rush_hour
```

#### Spatial

```text
origin
destination
distance
zone
region
```

#### Historical

```text
historical_mean
historical_median
historical_std
historical_percentiles
```

#### Weather

```text
temperature
precipitation
wind
weather_condition
```

### Deliverables

```text
notebooks/
reports/
feature definitions
EDA report
feature pipeline
```

### Definition of Done

The project has a documented understanding of the data and a reproducible feature-generation pipeline.

---

# Phase 4 — ML Baselines

### Objective

Build simple models before attempting sophisticated ML.

The purpose is to establish a measurable baseline.

---

## 4.1 Travel-Time Prediction

### Problem

Regression.

### Target

```text
travel_duration_minutes
```

### Baselines

```text
Historical Mean
Historical Median
Linear Regression
```

### Candidate models

```text
Random Forest
Gradient Boosting
XGBoost
```

### Metrics

```text
MAE
RMSE
MAPE where appropriate
```

---

## 4.2 Delay Prediction

### Problem

Binary classification.

### Initial target

```text
delay > 10 minutes
```

### Models

```text
Majority Class
Logistic Regression
Random Forest
XGBoost
```

### Metrics

```text
Precision
Recall
F1
ROC-AUC
PR-AUC
Calibration
```

---

## Deliverables

```text
Baseline models
Evaluation reports
Model comparison
Feature importance
Saved model artifacts
```

### Definition of Done

There is a reproducible baseline against which every later model can be compared.

---

# Phase 5 — Advanced ML & Uncertainty

### Objective

Improve prediction quality and, more importantly, estimate uncertainty.

FlowLens should not only answer:

> "What is the predicted travel time?"

It should also answer:

> "How uncertain is that prediction?"

---

## Candidate models

```text
XGBoost
LightGBM
CatBoost
Gradient Boosting
Quantile Regression
```

---

## Prediction output

Instead of:

```text
84 minutes
```

aim toward:

```text
Expected:
84 minutes

Prediction interval:
76–97 minutes
```

---

## Evaluate

```text
Point prediction accuracy
Prediction interval coverage
Interval width
Calibration
Error distribution
```

### Definition of Done

The system produces both travel-time predictions and meaningful uncertainty estimates.

---

# Phase 6 — Risk & Reliability Engine

### Objective

Transform ML predictions into understandable journey risk.

The engine combines:

```text
Travel-time prediction
+
Historical variability
+
Delay probability
+
Prediction uncertainty
```

into a reliability assessment.

---

## Outputs

```text
Expected travel time
Delay probability
Prediction interval
Reliability
Risk factors
```

### Example

```text
Expected:
84 minutes

Likely range:
76–97 minutes

Delay probability:
27%

Reliability:
Low / Medium / High
```

The exact reliability formulation must be validated rather than arbitrarily chosen.

---

## Important principle

Reliability is initially a **derived analytical metric**, not necessarily a separate ML model.

Later versions may learn reliability directly if experimentation demonstrates that this improves the system.

### Definition of Done

Given a model prediction, FlowLens can calculate and explain journey risk consistently.

---

# Phase 7 — Recommendation & What-If Engine

### Objective

Convert predictions into actionable decisions.

---

# 7.1 Recommendation Engine

Initially use an interpretable scoring/ranking system.

Potential factors:

```text
Expected duration
Delay probability
Reliability
Cost
Walking
Transfers
User preferences
```

Conceptually:

```text
User Preferences
       +
Plan Characteristics
       +
Predicted Risk
       ↓
Plan Score
       ↓
Ranked Plans
```

Do not start with reinforcement learning.

---

# 7.2 Departure Recommendation

Evaluate several possible departure times.

Example:

```text
7:30
7:45
8:00
8:15
8:30
```

For each:

```text
Expected travel time
Delay probability
Arrival time
Reliability
```

Then determine which departure windows satisfy the user's constraints.

---

# 7.3 What-If Engine

Initial scenarios:

```text
Leave earlier
Leave later
Change priority
```

Future scenarios:

```text
Weather changes
Route changes
Transit disruption
```

### Definition of Done

A user can modify a journey parameter and receive updated predictions and recommendations.

---

# Phase 8 — Backend & Database

### Objective

Turn the ML system into an accessible service.

### Backend

```text
FastAPI
Python
```

### Database

```text
PostgreSQL
PostGIS
```

---

## Conceptual entities

```text
Users
Preferences
Locations
Trips
Predictions
Routes
Weather
Recommendations
Model Versions
```

---

## API

Initial endpoints:

```text
GET  /api/v1/health

POST /api/v1/predict

POST /api/v1/recommend

POST /api/v1/what-if
```

Additional endpoints will be introduced as required.

---

## Architecture

```text
React
  ↓
FastAPI
  ↓
Service Layer
  ↓
ML / Risk / Recommendation
  ↓
PostgreSQL + PostGIS
```

### Definition of Done

The core FlowLens functionality can be accessed through documented APIs.

---

# Phase 9 — Frontend & User Experience

### Objective

Build the user-facing FlowLens application.

### Technology

```text
React
Tailwind CSS
```

---

## Screens

### Landing

Explain the FlowLens concept.

### Journey Planner

```text
Origin
Destination
Arrival/departure time
Mode
Preferences
```

### Analysis

Display:

```text
Expected duration
Delay probability
Travel-time range
Reliability
```

### Recommendations

Display alternative plans.

### What-if

Allow users to modify departure time and compare outcomes.

---

## UX principle

The interface should emphasize uncertainty and decision support.

Instead of only:

```text
ETA: 84 minutes
```

display:

```text
84 min expected

76–97 min likely range

27% delay risk
```

### Definition of Done

A user can complete the MVP workflow entirely through the frontend.

---

# Phase 10 — End-to-End Integration

### Objective

Connect the entire system.

```text
USER
 ↓
React
 ↓
FastAPI
 ↓
Feature Generation
 ↓
ML Model
 ↓
Risk Engine
 ↓
Recommendation Engine
 ↓
What-if Engine
 ↓
React
```

### Tasks

* Connect frontend to API
* Connect API to ML inference
* Connect database
* Validate input
* Handle errors
* Add loading states
* Add prediction response schema
* Add recommendation response schema
* Add what-if response schema

### Definition of Done

The MVP operates end-to-end without manual notebook intervention.

---

# Phase 11 — Explainability & AI Layer

### Objective

Make model outputs understandable.

The AI layer is **not the prediction engine**.

Instead:

```text
ML
 ↓
Structured Results
 ↓
Risk Factors
 ↓
AI Explanation
```

Example:

```text
Why is this journey risky?

• Morning peak period
• High historical travel-time variability
• Elevated precipitation
```

The explanation system must be grounded in actual model outputs and available data.

---

## Potential AI features

```text
Natural-language explanation
Journey summary
Risk explanation
What-if explanation
```

### Definition of Done

Users can understand the reasoning behind a prediction or recommendation without replacing the underlying quantitative analysis.

---

# Phase 12 — Testing & Evaluation

### Objective

Establish whether FlowLens actually works.

---

# ML Evaluation

## Regression

```text
MAE
RMSE
MAPE
```

## Classification

```text
Precision
Recall
F1
ROC-AUC
PR-AUC
Calibration
```

## Uncertainty

```text
Coverage
Interval width
Calibration
```

---

# System Evaluation

Measure:

```text
API latency
Inference latency
Data pipeline reliability
Error handling
Recommendation consistency
```

---

# Product Evaluation

Test:

```text
Does the recommendation satisfy arrival constraints?

Does risk information change appropriately
when departure time changes?

Are recommendations consistent with user priorities?

Are uncertainty estimates useful?
```

### Definition of Done

The project contains quantitative evidence demonstrating the performance and limitations of FlowLens.

---

# Phase 13 — Docker & Deployment

### Objective

Package the system for reproducible deployment.

### Dockerize

```text
Frontend
Backend
ML service
Database
```

Potential architecture:

```text
             Cloud
               │
       ┌───────┴────────┐
       │                │
   Frontend          Backend
                         │
              ┌──────────┼──────────┐
              ↓          ↓          ↓
             ML       Database    Cache
```

---

## Learn and implement

```text
Docker
Docker Compose
Environment variables
Container networking
Volumes
Health checks
Production builds
```

---

## Cloud

AWS can be introduced after the application works locally.

Potential services can be selected based on the final architecture rather than prematurely committing to every AWS service.

### Definition of Done

A clean deployment can reproduce the FlowLens application outside the development environment.

---

# Phase 14 — Productionization

### Objective

Make FlowLens robust enough for sustained usage.

Potential improvements:

```text
Authentication
Authorization
Rate limiting
Caching
Logging
Monitoring
Error tracking
Model versioning
Data versioning
Scheduled retraining
Automated data validation
CI/CD
```

---

## ML lifecycle

```text
Data
 ↓
Training
 ↓
Validation
 ↓
Model Registry
 ↓
Deployment
 ↓
Monitoring
 ↓
Retraining
```

### Definition of Done

The system has a reproducible and maintainable ML/software lifecycle.

---

# Phase 15 — Advanced FlowLens / V2+

Only begin these features after the MVP is stable.

---

# V2 — Real-Time Mobility Intelligence

Potential inputs:

```text
Real-time traffic
Real-time transit
Weather forecasts
Transit disruptions
Road incidents
Events
```

Potential features:

```text
Live risk updates
Dynamic departure recommendations
Journey monitoring
Disruption alerts
Real-time what-if analysis
```

---

# V2.5 — Mobility Anomaly Detection

Detect when current mobility conditions significantly differ from historical patterns.

```text
Historical Pattern
       ↓
Expected Mobility
       ↓
Current Observations
       ↓
Deviation
       ↓
Anomaly
```

Potential applications:

```text
Unusual congestion
Transit disruption
Event-related mobility changes
Weather-related anomalies
```

The system should distinguish anomaly detection from confidently identifying the cause.

---

# V3 — Personalized Mobility Intelligence

Potential features:

```text
Personal commute history
Personalized travel-time models
Personal risk tolerance
Personalized recommendations
Habit learning
Recurring journey analysis
```

---

# V4 — City Mobility Intelligence

Long-term expansion beyond individual users.

Potential applications:

```text
Mobility corridor analysis
Demand analysis
Congestion pattern analysis
Disruption intelligence
Urban planning analytics
Research dashboards
```

Potential users:

```text
Commuters
Researchers
Urban planners
Mobility organizations
Transportation organizations
```

---

# Feature Prioritization

Features should be evaluated using:

```text
User value
+
Data availability
+
Technical feasibility
+
ML value
+
Novelty
```

A feature should not be added simply because it sounds impressive.

---

# MVP Feature Boundary

The first complete product should contain only:

```text
┌──────────────────────────────────────┐
│             FLOWLENS MVP             │
├──────────────────────────────────────┤
│                                      │
│  Journey Input                       │
│      ↓                               │
│  Travel-Time Prediction              │
│      ↓                               │
│  Delay Probability                   │
│      ↓                               │
│  Uncertainty                         │
│      ↓                               │
│  Reliability                         │
│      ↓                               │
│  Departure Recommendation            │
│      ↓                               │
│  Alternative Plans                   │
│      ↓                               │
│  What-If Analysis                    │
│                                      │
└──────────────────────────────────────┘
```

Everything else should be treated as an extension.

---

# Phase Dependencies

The project should follow these dependencies.

```text
Product Definition
       ↓
Data
       ↓
EDA
       ↓
Baseline ML
       ↓
Advanced ML
       ↓
Risk
       ↓
Recommendation
       ↓
Backend
       ↓
Frontend
       ↓
Integration
       ↓
Explainability
       ↓
Testing
       ↓
Deployment
       ↓
Productionization
```

Avoid implementing later layers before their dependencies are validated.

---

# Development Philosophy

## 1. Build the simplest valid system first

Do not start with the most complicated model.

```text
Baseline
   ↓
Measure
   ↓
Improve
   ↓
Measure again
```

---

## 2. Separate prediction from decision-making

```text
Prediction
"What is likely to happen?"

        ↓

Risk Analysis
"How uncertain/risky is it?"

        ↓

Recommendation
"What options satisfy the user's priorities?"
```

---

## 3. Data before architecture complexity

Do not introduce complex infrastructure before understanding the data.

---

## 4. Real-time comes after historical modeling

First prove:

```text
Historical Data
       ↓
Prediction
       ↓
Risk
```

Then introduce:

```text
Real-Time Data
       ↓
Dynamic Prediction
       ↓
Dynamic Risk
```

---

## 5. AI comes after quantitative intelligence

The LLM should explain or interact with the system.

It should not replace the statistical/ML prediction pipeline.

---

# Master Definition of Done

FlowLens is considered a complete MVP when:

```text
[ ] User can enter a journey
[ ] System generates required features
[ ] ML model predicts travel time
[ ] System estimates delay probability
[ ] System estimates uncertainty
[ ] Reliability is calculated
[ ] User preferences are incorporated
[ ] Alternative plans can be compared
[ ] Departure windows can be evaluated
[ ] What-if analysis works
[ ] Backend exposes the functionality
[ ] Frontend consumes the API
[ ] Database stores required information
[ ] System works end-to-end
[ ] ML performance is evaluated
[ ] System limitations are documented
[ ] Application can be deployed
```

---

# Final Project Evolution

```text
                         FLOWLENS
                            │
                            ▼
                    ┌───────────────┐
                    │      MVP      │
                    ├───────────────┤
                    │ Prediction    │
                    │ Delay Risk    │
                    │ Reliability   │
                    │ Planning      │
                    │ What-if       │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │      V1       │
                    ├───────────────┤
                    │ Context      │
                    │ Weather      │
                    │ Explanations │
                    │ Personalize  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │      V2       │
                    ├───────────────┤
                    │ Real-time     │
                    │ Alerts        │
                    │ Transit       │
                    │ Incidents     │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │     V2.5      │
                    ├───────────────┤
                    │ Anomaly       │
                    │ Detection     │
                    │ Disruptions   │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │      V3       │
                    ├───────────────┤
                    │ City Mobility │
                    │ Intelligence  │
                    │ Platform      │
                    └───────────────┘
```

---

# Current Status

```text
Phase 1 — IN PROGRESS
```

Completed:

```text
[x] Repository created
[x] Project structure created
[x] Product specification created
[x] Development roadmap created
```

Remaining:

```text
[ ] ML design
[ ] Data strategy
[ ] System architecture
[ ] Database design
[ ] API design
[ ] User flow
[ ] Visual architecture diagrams
[ ] Final README
```

