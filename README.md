
# Data Jobs Analysis

## Introduction
Welcome to the Data Jobs Analysis project! This project focuses on exploring and analyzing data related to job positions in the field of data, including roles such as Data Engineer, Data Scientist, Data Analyst, and more. The dataset used for this analysis contains information about job titles, categories, salaries, employee residence, experience levels, and other relevant details.

## Questions Explored
1. [Most Populated Job Titles](#most-populated-job-titles): What are the top 10 most populated job titles in the dataset?
2. [Highest Paying Job Categories](#highest-paying-job-categories): Which job categories have the highest average salaries?
3. [Number of Job Titles in Data Science](#number-of-job-titles-in-data-science): How many job titles in the dataset fall under the Data Science category?
4. [Highest Paying Salaries by Countries](#highest-paying-salaries-by-countries): What are the countries with the highest average salaries?
5. [Top 10 Countries with the Highest Salaries](#top-10-countries-with-the-highest-salaries): Can we identify the top 10 countries offering the highest salaries?
6. [Distribution of Job Categories](#distribution-of-job-categories): How are job categories distributed in the dataset?
7. [Salary Groups](#salary-groups): How can we categorize salary ranges based on specific bins?
8. [World Map - Average Salary](#world-map---average-salary): Can we visualize the average salary on a world map?

## Table of Contents
1. [Import Libraries Used](#import-libraries-used)
2. [Read Data](#read-data)
3. [Data Cleaning](#data-cleaning)
4. [Data Analysis](#data-analysis)
    - [Most Populated Job Titles](#most-populated-job-titles)
    - [Highest Paying Job Categories](#highest-paying-job-categories)
    - [Number of Job Titles in Data Science](#number-of-job-titles-in-data-science)
    - [Highest Paying Salaries by Countries](#highest-paying-salaries-by-countries)
    - [Top 10 Countries with the Highest Salaries](#top-10-countries-with-the-highest-salaries)
    - [Distribution of Job Categories](#distribution-of-job-categories)
    - [Salary Groups](#salary-groups)
    - [World Map - Average Salary](#world-map---average-salary)

## Import Libraries Used
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import plotly_express as px
```
## Read Data
```python

df = pd.read_csv(r"C:\Users\new user\OneDrive\Desktop\Talusba\Python\jobs_in_data.csv")
```
## Data Cleaning
```python

# Check for null values
df.isnull().sum()

# Check for duplicates
df.duplicated().sum()

# Drop duplicates
df.drop_duplicates(inplace=True)

# Drop unnecessary columns
df = df.drop(columns=['salary', 'salary_currency'])
```
## Data Analysis
### Most Populated Job Titles
```python

MP = df['job_title'].value_counts()
MP_ = MP[:10]
MP_.plot(kind='barh', figsize=(20, 10))
plt.title("Most Populated Job Titles", fontsize=20)
plt.show()
```
Answer: The top 10 most populated job titles include Data Engineer, Data Scientist, Data Analyst, and more.

### Highest Paying Job Categories
```python
category_salary = df.groupby('job_category')['salary_in_usd'].mean().sort_values(ascending=False)
category_salary.plot(kind='bar')
plt.title("Highest Paying Job Categories", fontsize=20)
```
**Answer**: Machine Learning and AI, Data Science and Research, and Data Architecture and Modeling are the job categories with the highest average salaries.

### Number of Job Titles in Data Science
```python

job_title_count = len(MP)
job_title_count
```
**Answer**: There are 125 job titles related to Data Science in the dataset.

### Highest Paying Salaries by Countries
```python
residence_salary = df.groupby('employee_residence')['salary_in_usd'].mean().sort_values(ascending=False)
residence_salary[:10]
```
**Answer**: Qatar, Malaysia, and Puerto Rico are the countries with the highest average salaries.

### Top 10 Countries with the Highest Salaries
```python
sns.barplot(x=residence_salary.nlargest(10).index, y=residence_salary.nlargest(10).values)
plt.xticks(rotation=45)
plt.title("Top 10 Countries with the Highest Salaries", fontsize=20)
```
**Answer**: The top 10 countries with the highest salaries include Qatar, Malaysia, and Puerto Rico.

### Distribution of Job Categories
```python

fig = px.bar(df.job_category.value_counts().reset_index(name='Count'),
             x='job_category',
             y='Count',
             title='Distribution of Job Categories')
fig.show()
```
**Answer**: The distribution of job categories shows the frequency of each category in the dataset.

### Salary Groups
```python

df['salary_usd_group'] = pd.cut(df.salary_in_usd,
                                bins=[0, 50_000, 100_000, 200_000,
                                      300_000, 400_000, 500_000],
                                labels=['0-50K', '50k-100k', '100k-200k',
                                        '200k-300k', '300k-400k', '400k-500k'])
df.head(3)
```
**Answer**: The salaries have been categorized into groups (0-50K, 50k-100k, etc.) based on specific bins.

### World Map - Average Salary
```python

fig = px.choropleth(df.groupby('employee_residence')['salary_in_usd'].mean().reset_index(name='Average Salary'), 
                    locations='employee_residence',
                    locationmode='country names',
                    color='Average Salary',
                    hover_name='employee_residence',
                    color_continuous_scale='plasma')
fig.update_geos(projection_type="natural earth", showcoastlines=True)
fig.update_layout(title='World Map - Average Salary')
fig.show()
```
**Answer**: The world map visualizes the average salary across different countries.
