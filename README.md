# AI-Driven Personalised Fitness Coach

An intelligent system that generates, optimises, and adapts personalised weekly workout plans using a combination of classical AI search/optimisation techniques and machine learning — built as a team project.

> This repo is my personal copy of a team project.

## What It Does

Most generic workout plans ignore individual differences in fitness level, recovery capacity, and goals, often leading to poor results or injury. This project builds an adaptive fitness coach that takes in a user's profile (age, weight, height, activity level, weekly workout count) and produces a personalised, injury-aware weekly workout plan through the following pipeline:

1. **Exploratory Data Analysis** — Fitbit-style activity and sleep data are cleaned, merged, and visualised (step distribution, calories vs. steps, sleep distribution, activity-level breakdown, sleep vs. activity correlation).
2. **User Profiling (KNN)** — A K-Nearest Neighbors classifier predicts the user's ideal plan category (weight loss / muscle gain / endurance) from their profile features.
3. **Fitness-Level Clustering (K-Means)** — Users are grouped into beginner / intermediate / advanced archetypes.
4. **Weekly Schedule Generation (Genetic Algorithm)** — A population of candidate 7-day schedules is evolved (tournament selection, crossover, mutation) to optimise goal alignment, muscle-group balance, and rest.
5. **Exercise Selection (Graph Search: A\* / BFS)** — Exercises for each workout day are chosen by searching an exercise-dependency graph with a heuristic that prioritises full muscle-group coverage.
6. **Weekly Scheduling Demo (CSP)** — A standalone constraint satisfaction model assigns workout types to days of the week, as an alternative illustration of scheduling.
7. **Injury Risk Assessment (Bayesian Network)** — A Bayesian Network (via `pgmpy`) estimates injury risk (Low / Medium / High) from training volume, sleep, and soreness, and automatically trims the plan if risk is high.
8. **Interactive Interface (Gradio)** — A Gradio app lets a user enter their profile and get a generated plan, predicted goal/fitness level, and injury risk in real time.

## Technologies Used

- **Python 3.x**
- **NumPy / Pandas** — data handling and feature engineering
- **Matplotlib / Seaborn** — EDA visualisations
- **Scikit-learn** — KNN classifier, K-Means clustering, preprocessing, train/test split
- **python-constraint** — CSP scheduling demo
- **pgmpy** — Bayesian Network construction and inference
- **Gradio** — interactive web interface

## Project Structure

```
.
├── main.ipynb                 # Full pipeline: EDA → ML → GA → graph search → Bayesian network → Gradio UI
├── dailyActivity_merged.csv   # Input: daily activity data
├── sleepDay_merged.csv        # Input: daily sleep data
└── README.md
```

## Setup & Usage

1. Place `dailyActivity_merged.csv` and `sleepDay_merged.csv` in the same folder as `main.ipynb`.
2. Install dependencies:
   ```
   pip install numpy pandas matplotlib seaborn scikit-learn pgmpy python-constraint gradio
   ```
3. Run all cells in `main.ipynb` top to bottom. Optional packages (`python-constraint`, `pgmpy`, `gradio`) are auto-installed by the notebook if missing.
4. The final cell launches a Gradio app for entering your profile and generating a plan.

## Output

- A personalised 7-day workout plan
- Predicted plan category and fitness archetype
- Injury risk score (Low / Medium / High) with automatic plan adjustment
- GA convergence chart (best/average fitness per generation)
- EDA dashboard of activity and sleep trends

## My Contributions

This was a three-person team project. My main contribution was the **data preparation and exploratory data analysis (EDA)** stage of the pipeline:

- Cleaned and merged the raw activity and sleep datasets (parsing dates, dropping duplicates)
- Engineered features used downstream (e.g. active minutes, calorie categories)
- Built the EDA visualisations: daily step distribution, calories-burned-vs-steps scatter plot, sleep-hours distribution, activity-level breakdown, and the sleep-vs-active-minutes correlation chart
- Produced the summary statistics tables for activity and sleep data
- Debugged and fixed errors in the Genetic Algorithm and KNN classifier implementations
- Polished the Gradio UI (layout, input/output presentation) for the final interface
