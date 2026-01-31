# Instagram-Reel-Analytics-My-Account
# Instagram Reel Performance Analytics 📊  
**Business vs Motivation Content | Real Creator Data**

---


## 📌 Project Overview
This project is an **end-to-end data analytics study** performed on **my own Instagram page** (80K+ followers), focused on understanding how **content type, posting time, engagement, and watch behavior** influence **reach and follower growth**.

The project follows a **realistic analytics workflow**, starting from platform data extraction to dashboard-driven exploration, without artificial assumptions or overengineering.

---

## 🎯 Business Objective
To analyze Instagram Reel performance and explore relationships between:
- Reach & visibility  
- Engagement quality  
- Watch behavior (retention)  
- Follower growth  

with the goal of deriving **data-backed content strategy insights** for a creator account.

---

## 🗂️ Data Source
- **Primary Source:** Instagram Insights exported using **Meta Business Suite**
- **Account Type:** Professional account (Creator)
- **Granularity:** Reel-level data (~70 reels)
- **Content Types:**
  - Business
  - Motivation

### Manually Added Metrics
Due to limitations in Meta Business Suite exports:
- `Average Play Time`
- `Total Watch Time`

were **manually recorded per reel** from Instagram Insights to ensure accuracy.

---

## 🧹 Data Cleaning & Feature Engineering (Excel)

All cleaning and feature engineering was intentionally done in **Excel** to reflect a realistic fresher-level analytics workflow.

### Cleaning Steps
- Converted publish timestamps into datetime format
- Ensured numeric consistency across reach, engagement, and watch metrics
- No rows were removed to preserve original platform data

### Derived Features
- `Post Hour` – extracted from publish time  
- `Post Day` – extracted from publish date  

### Derived Metrics
```text
Engagement Rate = (Likes + Comments + Shares + Saves) / Reach
Watch Retention Ratio = Average Play Time / Reel Duration

## DAX Measures (Power BI)

All KPIs in the dashboard were created using **DAX measures** (not calculated columns) to ensure dynamic behavior with filters and slicers.

---

### 📊 Reach & Visibility

#### Average Reach
Measures the average number of accounts reached per reel.

```DAX
Avg Reach =
AVERAGE ( instagram_reels[Reach] )

Avg Engagement Rate =
AVERAGE ( instagram_reels[Engagement_Rate] )

Total Likes =
SUM ( instagram_reels[Likes] )

Total Comments =
SUM ( instagram_reels[Comments] )

Total Shares =
SUM ( instagram_reels[Shares] )

Avg Watch Retention =
AVERAGE ( instagram_reels[Watch_Retention_Ratio] )

Like Impact Score =
DIVIDE (
    SUM ( instagram_reels[Likes] ),
    AVERAGE ( instagram_reels[Average_Play_Time] )
) 
### Python Code

```python
import pandas as pd
df = pd.read_excel("../data/Instagram_reels.xlsx")
```

## Basic Metrics

```python
summary = df.groupby("Content Type").agg({
    "Reach": "mean",
    "Engagement Rate": "mean",
    "Retention Ratio": "mean",
    "Follows": "mean"
}).reset_index()

summary
```

## Watch Retention vs Follows

```python
df[["Retention Ratio", "Follows"]].corr()
```

## Top 25% Reels

```python
threshold = df["Reach"].quantile(0.75)
top_reels = df[df["Reach"] >= threshold]

top_reels.drop(columns=["Date"]).describe()
```

## Reach According to Content Type and Posting Time

```python
df.groupby(["Content Type", "Post Hour"])["Reach"].mean().reset_index()
```
