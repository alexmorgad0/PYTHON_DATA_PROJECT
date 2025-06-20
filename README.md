# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter EU Jobs

To focus my analysis on the EU job market, I apply filters to the dataset, narrowing down to roles based in the Europe Union.

```python
eu_countries = [
    'Austria', 'Belgium', 'Bulgaria', 'Croatia', 'Cyprus', 'Czechia',
    'Denmark', 'Estonia', 'Finland', 'France', 'Germany', 'Greece',
    'Hungary', 'Ireland', 'Italy', 'Latvia', 'Lithuania', 'Luxembourg',
    'Malta', 'Netherlands', 'Poland', 'Portugal', 'Romania', 'Slovakia',
    'Slovenia', 'Spain', 'Sweden'
]

df_DA_EU = df[(df['job_country'].isin(eu_countries)) & (df['job_title_short'] == 'Data Analyst')]

```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skills_Demand](5_Project/2_Skills_Demand.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Likelihood of Skills Requested in the EU Job Postings](5_Project\images\skill_demand.png)

*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights:

- SQL is the most requested skill for Data Analysts and Data Engineers in the EU, appearing in 45% and 55% of job postings respectively. For Data Scientists, it remains highly relevant at 38%.

- Python is the top skill for Data Scientists (63%) and Data Engineers (56%), and is also in demand for Data Analysts (31%), highlighting its cross-functional importance.

- Data Engineers require more specialized cloud and infrastructure skills such as Azure (35%), AWS (29%), and Spark (28%), compared to the other roles.

- Data Analysts are expected to be familiar with Excel (26%), Power BI (22%), and Tableau (20%), emphasizing business-oriented and reporting tools.

- Data Scientists show moderate demand for R (27%), Azure (14%), and SAS (13%), indicating a mix of statistical and enterprise tools.

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](5_Project/3_Skills_Trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_EU_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in the EU](5_Project\images\trending_top_skills_EU_2023.png)  
*Bar graph visualizing the trending top skills for data analysts in the EU in 2023.*

### Insights:
- SQL is the most in-demand skill for Data Analysts in the EU throughout the year, with a slight overall decline but maintaining a clear lead over other tools.

- Python holds steady as the second most requested skill, showing minor fluctuations but rising in December.

- Excel remains consistently important, with small dips mid-year but bouncing back near year-end.

- Power BI and Tableau have lower overall demand but follow a similar pattern—slight declines in the second half of the year, with a modest recovery in the last months.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in the Europe Union and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](5_Project/4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_EU_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in the EU](5_Project\images\salary_distribution_of_data_jobs_eu.png)  
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

- Salaries for data roles in the EU show clear increases with seniority. Senior Data Scientists and Senior Data Engineers have the highest median and upper-range salaries, confirming that experience and specialization significantly boost compensation.

- Data Analyst roles have the lowest median salaries and the narrowest interquartile ranges, suggesting more consistent and standardized pay across the EU for entry-level positions.

- Senior Data Analyst positions show a tighter salary band than other senior roles, implying limited salary progression compared to technical tracks like engineering and science.

- While Data Engineers and Data Scientists have overlapping salary distributions, engineers appear slightly better compensated at the median level, possibly reflecting demand for infrastructure-related skills.

- Outliers are present across several roles, particularly Data Engineer and Senior Data Engineer, hinting at niche or high-demand situations that lead to significantly higher offers.

### Highest Paid & Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

#### Visualize Data

```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Analystsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()

```

#### Results
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the EU:

![The Highest Paid & Most In-Demand Skills for Data Analysts in the EU](5_Project\images\highest_paid_skills_eu.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in the EU.*

#### Insights:

- The top graph reveals that specialized technical skills like c, terraform, mongo, and nosql are associated with the highest median salaries—exceeding $160K in some cases. These niche or backend-oriented tools signal premium compensation for technical depth in analytics.

- In contrast, the bottom graph shows that tools like sql, python, tableau, and excel dominate demand. These core data analysis skills are foundational across most job postings, despite offering lower median salaries.

- This divergence illustrates a key strategy: foundational tools help land jobs, while specialized or less common technologies command higher pay. To stand out in the EU market, aspiring data analysts may benefit from mastering high-demand tools while strategically adding high-paying tech skills to their toolbox.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_Optimal_Skills](5_Project/5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```
#### Results

![Most Optimal Skills for Data Analysts in the EU](5_Project\images\optimal_basic.png)    
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the EU.*

#### Insights:

- SQL and Python strike the best balance of high demand and solid compensation, appearing in over 35% of job postings with median salaries around $100K, making them essential baseline skills for data analysts in the EU.

- Cloud and big data tools like gcp, aws, spark, and azure offer top-tier salaries ($102K–$112K) despite being requested in fewer than 15% of postings, signaling high-pay potential for analysts with these specialized skills.

- While Excel is the most common tool across analyst roles in the EU, it offers one of the lowest median salaries (~$73K)—highlighting the trade-off between ubiquity and earning potential.

- To maximize both job opportunities and salary, EU data analysts should combine foundational skills (like SQL, Python) with higher-paying niche tools (like GCP, Looker, or AWS).

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

#### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in the EU with Coloring by Technology](5_Project\images\optimal_skills_EU.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the EU with color labels for technology.*

#### Insights:

- Programming tools like python and sql (blue) are highly demanded and offer consistently strong salaries near $100K, reinforcing their role as core technical skills for data analysts in the EU.

- Cloud skills (gcp, aws, azure) stand out for offering the highest median salaries (above $102K) while being requested in fewer jobs, suggesting a high-value niche for analysts who invest in cloud expertise.

- Analyst tools such as excel, power bi, and tableau are widely required and offer respectable pay, especially tableau which offers both reach and reward, while excel remains low-paying despite its high usage.

- Analysts seeking career optimization in the EU should build a foundation in widely-used tools (like SQL and Python), while layering in cloud or BI skills to stand out and boost earning potential.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.

# Insights

This project provided several general insights into the data job market for analysts:

- **Foundational Skills Dominate Demand:** Core tools like SQL, Python, and Excel are the most frequently requested across job postings, showing that foundational skills remain essential for employability in the EU market.

- **Specialized Skills Offer Salary Leverage:** Although less common, skills like GCP, Looker, and Terraform are associated with significantly higher salaries, indicating strong compensation potential for analysts who specialize.

- **Seniority Drives Salary Variability:** Roles like Senior Data Scientist and Senior Data Engineer show the widest salary ranges and highest medians, emphasizing that experience and seniority are strongly rewarded in the EU job market.

- **Cloud Skills Are High-Value and Underserved:** Skills in cloud technologies (AWS, GCP, Azure) are less common in job descriptions but consistently tied to top-tier salaries, suggesting an opportunity gap for EU-based analysts to fill.

- **Skill Optimization Is Key:** By aligning skills that are both in-demand and well-paid, such as SQL and Tableau, analysts can strategically boost their career prospects while maintaining versatility across roles.


# Challenges I Faced

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.

# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.