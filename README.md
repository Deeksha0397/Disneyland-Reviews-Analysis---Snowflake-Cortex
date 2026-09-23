# Disneyland Reviews Analysis — Snowflake Cortex

## Project Overview

This project analyzes thousands of Disneyland guest reviews using **Snowflake, Snowflake Cortex AI functions, SQL, Python, and Streamlit**.

The main goal is to convert unstructured customer review text into structured, measurable insights such as sentiment, review categories, locations, branches, and trends over time.

Instead of manually reading thousands of reviews or moving the text to an external system for every analysis, the project uses **Snowflake Cortex** to perform AI-powered text processing directly within Snowflake.

---

## Business Problem

Disneyland receives a large volume of guest reviews covering areas such as:

- Rides & Attractions
- Events & Entertainment
- Atmosphere & Magic
- Staff & Service
- Pricing & Value
- Food & Dining
- Overcrowding & Queues
- Cleanliness & Maintenance

Manually reviewing thousands of comments to identify patterns would be time-consuming.

This project provides a data-driven approach to:

- Understand overall guest sentiment
- Identify positive and negative experiences
- Find frequently discussed categories
- Compare sentiment across Disneyland branches
- Analyze sentiment by reviewer location
- Track sentiment trends over time
- Identify areas that may require operational improvement

---

## Snowflake Cortex Functions Used

### 1. TRANSLATE

Used to translate review text into a target language when reviews are written in different languages.

Example use case:

```sql
SELECT SNOWFLAKE.CORTEX.TRANSLATE(
    review_text,
    'en',
    'fr'
) AS translated_review
FROM reviews;
```

This helps create a common language representation for further analysis.

---

### 2. SUMMARIZE

Used to summarize long review text and extract the main information from a review.

This is useful when a review contains a large amount of text but the analysis requires a concise representation.

---

### 3. SENTIMENT

Used to calculate the sentiment of a review.

The function returns a numerical sentiment score where:

- Negative values indicate negative sentiment
- Values around zero indicate neutral sentiment
- Positive values indicate positive sentiment

The sentiment score can then be aggregated to understand customer experience at different levels.

---

### 4. AI_AGG

`AI_AGG` is used to analyze a group of text records together.

Instead of processing only one review at a time, a prompt can be applied to a collection of reviews to extract a higher-level answer or summary.

For example, it can help answer questions such as:

> What are customers saying about staff service?

or

> What are the major complaints mentioned in these reviews?

This is useful for group-level customer feedback analysis.

---

### 5. CLASSIFY_TEXT

Used to classify review text into predefined business categories.

For example:

```text
Review
  ↓
CLASSIFY_TEXT
  ↓
Category
```

Possible categories include:

- Rides & Attractions
- Events & Entertainment
- Atmosphere & Magic
- Staff & Service
- Pricing & Value
- Food & Dining
- Overcrowding & Queues
- Cleanliness & Maintenance

This converts free-form review text into structured categorical data.

---

### 6. AI_CLASSIFY

Used when multiple categories need to be assigned or when classification requires a more flexible AI-based approach.

The output can contain category information in a structured/JSON-like format.

When the output contains nested category information, **FLATTEN** can be used to convert it into rows for further SQL analysis.

---

## Data Processing Flow

```text
Disneyland Guest Reviews
          ↓
      Snowflake
          ↓
  Snowflake Cortex AI
          ↓
 ┌───────────────────────┐
 │ Translate             │
 │ Summarize             │
 │ Sentiment             │
 │ AI_AGG                │
 │ Classify Text         │
 │ AI Classify           │
 └───────────────────────┘
          ↓
 Structured / Enriched Data
          ↓
 SQL Aggregation & Analysis
          ↓
      Streamlit App
          ↓
 Interactive Dashboard
```

---

## JSON and FLATTEN Processing

Some AI classification results can be returned as structured JSON containing multiple categories or attributes.

For analysis, the nested output can be flattened:

```text
JSON Object
     ↓
   FLATTEN
     ↓
Individual Rows
     ↓
GROUP BY / COUNT
     ↓
Category-level Analysis
```

This makes it possible to calculate review counts and sentiment metrics for individual categories.

---

## Dashboard

The project includes a **Streamlit dashboard deployed from Snowflake**.

### 1. Average Sentiment Score per Branch

Compares average guest sentiment across Disneyland branches.

This helps identify differences in overall customer experience between branches.

---

### 2. Sentiment Distribution per Category

Shows the proportion of:

- Positive
- Negative
- Neutral
- Mixed

sentiment within each review category.

This helps identify categories where negative feedback is relatively more prominent.

---

### 3. Average Sentiment Score per Reviewer Location

Compares average sentiment based on the reviewer's location/country.

This provides a geographic view of customer feedback.

---

### 4. Review Count per Category

Shows the number of reviews associated with each category.

This helps identify which areas of the Disneyland experience receive the most customer feedback.

---

### 5. Average Sentiment Value Over Time

Shows how average review sentiment changes across years.

This can help identify periods where customer sentiment increased or decreased.

---

## Key Business Insights

The dashboard can be used to identify:

- Categories receiving large volumes of customer feedback
- Categories with comparatively higher negative sentiment
- Differences in customer sentiment across branches
- Differences in sentiment across reviewer locations
- Changes in customer sentiment over time
- Areas that may require operational attention

The analysis is intended to help management move from **manually reading reviews to data-driven customer experience monitoring**.

---

## Business Impact

The project can support Disneyland teams in:

### Customer Experience

Identify recurring customer concerns and positive experiences.

### Operations

Detect areas such as queues, cleanliness, food, or service that may require attention.

### Management

Compare branches and monitor customer sentiment over time.

### Customer Feedback

Turn large volumes of unstructured reviews into structured information that can be analyzed using SQL.

### Decision Making

Use sentiment and category-level analysis to prioritize areas for investigation and improvement.

---

## Streamlit Application

The Streamlit application provides an interactive way to visualize the processed Snowflake data.

The application uses Python/Snowpark to access Snowflake data and creates charts using tools such as **Altair**.

The dashboard includes interactive analysis rather than requiring users to manually run SQL queries for every insight.

---

## Example Analytical Questions

The project can answer questions such as:

1. Which category receives the most reviews?
2. Which categories have more negative feedback?
3. What is the average sentiment for each branch?
4. How does sentiment vary by reviewer location?
5. How has customer sentiment changed over time?
6. What are customers saying about a particular category?
7. What recurring issues appear in guest reviews?

---

## Why Snowflake Cortex?

A key advantage of this project is that AI-powered text analysis can be performed close to the data inside the Snowflake environment.

Traditional workflow:

```text
Snowflake
   ↓
Move data outside
   ↓
Python / External API
   ↓
Process text
   ↓
Move results back
```

Cortex-based workflow:

```text
Snowflake
   ↓
Snowflake Cortex
   ↓
AI Processing
   ↓
Structured Results
   ↓
SQL Analysis
```

This reduces unnecessary data movement and allows AI-generated results to be combined directly with SQL-based analytics.

---

## Conclusion

The **Disneyland Reviews Analysis** project demonstrates how Snowflake and Snowflake Cortex can be used to transform unstructured customer feedback into structured business insights.

By combining:

**Snowflake + Cortex AI + SQL + Python + Streamlit**

the project creates an end-to-end analytics workflow for understanding customer sentiment, categorizing reviews, identifying patterns, and visualizing guest experience trends.

---

## Skills Demonstrated

- SQL Analytics
- Snowflake
- Snowflake Cortex
- AI-powered Text Analysis
- Sentiment Analysis
- Text Classification
- JSON Processing
- `FLATTEN`
- Aggregation and Grouping
- Python
- Snowpark
- Pandas
- Streamlit
- Data Visualization
- Business Insight Generation
