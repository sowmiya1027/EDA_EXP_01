# Experiment 1: EDA in IPL Dataset
## Name: Sowmiya R
## Register No: 212225040420

## Aim:
To perform Exploratory Data Analysis (EDA) on the IPL matches dataset and derive insights about matches per season, winning teams, toss decisions, and top venues.

## Algorithm / Procedure:

## 1.Import Libraries
  Import pandas for data handling.
  Import matplotlib and seaborn for visualization.
## 2.Load Dataset
  Use pd.read_csv() to load the IPL matches dataset.
  Check dataset shape using .shape.
  View first 5 rows using .head().
## 3.Matches per Season (Univariate Analysis)
  Group data by season and count matches.
  Plot a bar chart to visualize growth/decline in matches.
## 4.Top Winning Teams (Univariate Analysis)
  Use value_counts() on the winner column.
  Plot top 5 winning teams in a bar chart.
## 5.Toss Decisions (Univariate Analysis)
  Count toss decision preferences (bat vs field).
  Plot results using a bar chart.
## 6.Top Venues (Univariate Analysis)
  Count matches per venue.
  Display top 5 venues with a horizontal bar chart.
## 7.Draw Insights
  Observe patterns in toss decisions.
  Identify teams with consistent winning trends.
  
## Program
### Import and Load Dataset:
```
import pandas as pd
# Load the IPL dataset
df = pd.read_csv("matches.csv")
```

### 1. Dataset Understanding:
```
print("\nQ1. DATASET UNDERSTANDING")
print("Rows:", df.shape[0])
print("Columns:", df.shape[1])
print(df.head())
```

### 2. Columns and data types:
```
print("\nQ2. COLUMNS AND DATA TYPES")
print(df.dtypes)
```

### 3. Unique Identifier:
```
print("\nQ3. UNIQUE IDENTIFIER")
print("Unique IDs:", df["id"].nunique())
print("Is ID unique:", df["id"].is_unique)
```

### 4. Data Cleaning – Missing Values & Duplicates:
```
print("\nQ4. DATA CLEANING")
print("Missing values:")
print(df.isnull().sum()[df.isnull().sum() > 0])
print("Duplicate records before cleaning:", df.duplicated().sum())

df["city"] = df["city"].fillna("Unknown")
df = df.drop_duplicates().copy()

print("Missing city values after cleaning:", df["city"].isnull().sum())
print("Duplicate records after cleaning:", df.duplicated().sum())
```

### 5. GroupBy – Season Analysis:
```
print("\nQ5. SEASON ANALYSIS")
season_matches = df.groupby("season").size()
print(season_matches)
print("Season with most matches:", season_matches.idxmax())
print("Highest number of matches:", season_matches.max())
```

### 6. GroupBy & Sorting – Team Performance:
```
print("\nQ6. TOP 5 WINNING TEAMS")
team_wins = (
    df.dropna(subset=["winner"])
      .groupby("winner")
      .size()

      .sort_values(ascending=False)
)
print(team_wins.head(5))
```

### 7. Filtering – Team Analysis:
```
print("\nQ7. CSK MATCHES")
csk = df[df["winner"] == "Chennai Super Kings"]
print(csk[["season", "team1", "team2", "winner"]].head(10))
print("Total CSK wins:", len(csk))
```

### 8. Value Counts – Toss Analysis:
```
print("\nQ8. TOSS DECISION ANALYSIS")
toss_count = df["toss_decision"].value_counts()
toss_percent = df["toss_decision"].value_counts(normalize=True) * 100
print("Frequency:")
print(toss_count)
print("Percentage:")
print(toss_percent.round(2))
print("Most preferred:", toss_count.idxmax())
```

### 9. Cross-Tabulation – Season-wise Toss Analysis:
```
print("\nQ9. SEASON-WISE TOSS ANALYSIS")
toss_season = pd.crosstab(df["season"], df["toss_decision"])
print(toss_season)
print("Most preferred decision by season:")
print(toss_season.idxmax(axis=1))
```

### 10. GroupBy & Sorting – Venue Analysis:
```
print("\nQ10. TOP 5 VENUES")
venues = df.groupby("venue").size().sort_values(ascending=False)
print(venues.head(5))
```

### 11. Aggregation & Sorting – Winning Margin Analysis:
```
print("\nQ11. WINNING MARGIN ANALYSIS")
margin_df = df.dropna(subset=["result_margin"])
largest = margin_df.loc[margin_df["result_margin"].idxmax()]
print("Largest winning margin:", largest["result_margin"])
print(largest[[
    "season", "team1", "team2", "winner",
    "result", "result_margin"
]])

print("\nTop 10 winning margins:")
print(
    margin_df.sort_values("result_margin", ascending=False)
    [["season", "team1", "team2", "winner", "result_margin"]]
    .head(10).to_string(index=False)
)
```

### 12. Match result frequency:
```
print("\nQ12. MATCH RESULT ANALYSIS")
print(df["result"].value_counts())
```

### 13. Data Transformation – Datetime Conversion:
```
print("\nQ13. DATETIME CONVERSION")
print("Before conversion:", df["date"].dtype)
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("After conversion:", df["date"].dtype)
```

### 14. Data Transformation – Year Extraction:
```
print("\nQ14. YEAR EXTRACTION")
df["year"] = df["date"].dt.year
print(df[["date", "year"]].head(10).to_string(index=False))
```

### 15. Data Transformation – Column Creation:
```
print("\nQ15. WIN TYPE CREATION")
win_type_map = {
    "runs": "Won by Runs",
    "wickets": "Won by Wickets",
    "tie": "Tie",
    "no result": "No Result"
}
df["win_type"] = df["result"].map(win_type_map).fillna("Other")
print(df[["result", "win_type"]].head(10).to_string(index=False))
```

### Save the cleaned dataset:
```
df.to_csv("IPL_Matches_Cleaned.csv", index=False)
print("\nCleaned dataset saved as IPL_Matches_Cleaned.csv")
```


## Output
### Dataset Understanding:

<img width="952" height="710" alt="image" src="https://github.com/user-attachments/assets/1d88bca7-b89e-4267-a3de-49743b55267c" />


### Columns and Data types:

<img width="690" height="468" alt="image" src="https://github.com/user-attachments/assets/df3cd579-00b6-426f-9ddc-f39dfaf63d2e" />

  

 ## Result
  This experiment is executed successfully.
