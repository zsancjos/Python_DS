# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find  optimal job oppoertunities for data analysts.
The data source from Luke Barousse's Python Course which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.


# The Questions

Below are the questions I want to answer in my project:
    
1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)


# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of sevveral key tools:

* Python: The Backbone of my analysis, allowing me to analyze the data and fine critical insights.
    *   Pandas Library: The Python library used to analyze the data.
    *   Matplotlib Library: The library I used to visualize my data.
    *   Seaborn Library: The library I used to create more advanced visuals.

* Jupyter Notbooks: the tool I used to run my Python scripts which let me easily include my notes and analysis.
* Visual Studio Code: My go-to for executing my Python scripts.
* Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data analysis, ensuring accuracy and usability.


### Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import pandas as pd 
from datasets import load_dataset
import ast
import matplotlib.pyplot as plt
import seaborn as sns

# Loading Data 
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

### Filter Mexico Jobs

To focus my analysis in the Mexico job market, I apply filters to the dataset, narrowing down to roles based in Mexico.

    df_Mex = df[df['job_country'] == 'Mexico']

# The Analysis

Each jupyter notebook for this project aimed at investigating specific aspects of the job amrket. Here's how I approached each question:

## 1. What the most demanded skills for the top 3 most popular data roles? 

I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top3 roles. This query highlights the most popular job titles and thier tio skills, showing which skills I should pay attention to depending on the role I'm targeting.

View my notebook with detailed steps here: [2_Skills_Count.ipynb](3_Project\2_Skills_Count.ipynb)


### Visualize Data

```python
fig, ax =plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```
### Results

![Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.](3_Project\images\skill_demand_all_data_roles.png)

### Insights:

* SQL is the most requested skill for Data Analysts and Data Engineers, with it in over half the job postings for both roles. For Data Scientists, Python is the most sought-after skill, appearing in 62% of job postings.
* Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scinetists who are expected to be proficient in more general data managment and analysis tools(Excel, Tableau).
* Python is a versatile skill, highly demanded across all three roles, but most prominently for both Data Scientists and Data Engineers (62%).
    
## 2. How are in-demand skills trending for Data Analysis?  

#### Visualize Data

```python

df_plot = df_DA_Mex_percent.iloc[:, :5]  # all rows, first 5 values
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in Mexico](3_Project\images\skill_trend_DA.png)

*Line graph visiualizing the trending top skills for data analysts in Mexico in 2023.*

### Insights: 
- SQL and Excel Dominate Job Postings Throughout the Year:
    - SQL consistently maintains its position as the top skill required, peaking around the mid-year (May-June) at approximately 50% likelihood in job postings. Despite minor fluctuations, it remains the most in-demand skill overall.
    - Excel follows closely, demonstrating a significant rise from February to April before stabilizing. It has a consistent demand throughout the year, typically trailing SQL.

- Python and Tableau Show Complementary Trends:

    - Python demonstrates notable variability, with a sharp increase in demand during March and April, peaking at approximately 30%, followed by a steady decline after July. This pattern suggests seasonality or specific industry trends affecting Python's demand.
    - Tableau exhibits a complementary trend, with its demand peaking around March but remaining lower than Python throughout the year. This indicates a moderate but steady interest in Tableau among employers.

- Power BI Lags but Exhibits Gradual Growth:

    - Power BI consistently shows the lowest demand compared to other skills. However, there is a slight upward trend in its demand toward the year's end, aligning with a broader interest in data visualization tools.


## 3. How well do jobs pay for Data Aalysts?

### Salary Analysis for Data Nerds

#### Visiualize Data

```python
sns.boxplot(data=df_Mex_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}k')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```
### Results
![Salary Distribution in Mexico](3_Project\images\salary_boxplot.png)

*Box plot visiualizing the salary ditribution for thhe top 6 data job titles.*

### Insights
- Senior Roles Have Narrower Salary Ranges and the Highest Median Salaries:
    - Senior Data Scientist and Senior Data Engineer roles show significantly compact interquartile ranges (IQR) compared to other roles, indicating less variability in salaries at the senior level.

- Data Scientist Has a Wide Salary Range:
    - There's a significant variation in Data Sceintist salaries, having the highest slary potential, with up to $200k, indicating the high vvalue placed on advanced data skills and experience in the industry.

- Data Analyst Is the Lowest-Paid Role with Limited Variation:
    - Data Analysts have the lowest median salary among all roles, however, those earning above the 80th percentile earn more than Machine Learning Engineers.


## 4. How well do jobs and skills pay for Data
### Highest Paid & Most Demanded Skills for Data

#### Visiualize Data 
```python

fig, ax = plt.subplots(2, 1)

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data ANalysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results

![The Highest Paid & Most In-Demand Skills for Data Analysts in the US](3_Project\images\Highest_Paid_and_Most_Demand_Skills_For_Data_Analysts_in_Mexico.png)

*Two separate bar graphs visiualizing the highest paid skills and most in-demand skills for data analysts in Mexico.*

#### Insights:
- The top graph shows specialized techinical skills like `scala`, `spark`, and `go` are associated with higher salaries, some reaching over $140k, suggesting that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `SQL`, and `Python` are the most sought after, reflecting their utility in data manipulation and analyis.
- These insights reveal a clear distinction between widely required foundational skills and specialized skills that command highter salaries, offering guidance for data analysts aiming to maximize their career potential.

## 4. What is the most optimal skill to learn for Data Analysts?

#### Visualize Data
```python
from adjustText import adjust_text
from matplotlib.ticker import PercentFormatter

sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

plt.show()

```

#### Results

![Most optimal Skills for Data Analysts in Mexico](3_Project\images\optimal_skills.png) 

*A scatter plot visiualizing the most optimal skills (high paying & high demand) for data analysts in Mexico.*

#### Insights:

- The scatter plot shows that most of the `programming` skills (colored orange) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

- Analysts tools (colored blue), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visialization and data analysis software are crucial for current data roles. This category no only has good salaries but is also versatile across different types of data tasks.


# What I learned
Throuhgout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visiualization. Here are a few specific things I learned:

- Advance Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visiualization, and other libraries helped me perform complex data analysis tasks more efficiently.

- Data Cleaning Importance: I learned that thorough data cleaning and preperation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.

- Strategic Skill Analysis: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship betweeen skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

- Skill Demand and Slaary Correlation: There is clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle foten lead to higher salaries.

- There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.

- Economic Value of Skills: Understanding which skills are both in-demand and well-compensated can guid data analysts in prioritizing learning to maximize their economic returns.

# Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:

- Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.

- Complex Data Visiualization: Designing effective visiual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.

- Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion
This  exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field, the insights I got enhanced my understanding and provide actionable guidance for anyone looking to advance their career in data anlaytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.