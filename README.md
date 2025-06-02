# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from Hugging Face ([https://lukebarousse.com/python](https://huggingface.co/datasets/lukebarousse/data_jobs)) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

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
!pip install --upgrade datasets fsspec
# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter German Jobs

To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in Germany.

```python
df_Germany = df[df['job_country'] == 'Germany']

```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles. I filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skill_Demand](2_Skill_Demand.ipynb).

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results

![Likelihood of Skills Requested in Germany Job Postings](jobskilldemand.png)

*Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.*

### Insights:

1. SQL and Python are highly in demand across all three roles — Data Analyst, Data Engineer, and Data Scientist — with Python being the most requested skill for Data Scientists (62%) and Data Engineers (53%), and SQL topping the list for Data Analysts (41%).

2. Data Analysts are more likely to be expected to know business intelligence tools like Tableau (19%), Excel (19%), and Power BI (18%), reflecting a focus on reporting and visualization.

3. Data Engineers need a broader tech stack, including cloud platforms like Azure (29%) and AWS (25%), along with tools like Spark (20%), indicating the importance of infrastructure and data pipeline management.

4. Data Scientists are also expected to know statistical programming (R - 27%) and some cloud skills (Azure - 14%, AWS - 11%), though to a lesser extent than Data Engineers.

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Skills_Trend.ipynb).

### Visualize Data

```python

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_Germany_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()

```

### Results

![Trending Top Skills for Data Analysts in Germany](Trendingtopskills.png)  
*Bar graph visualizing the trending top skills for data analysts in Germany in 2023.*

### Insights:
1. SQL maintained a leading presence throughout 2023, peaking around mid-year (~70% job mentions), proving it’s a must-have skill.

2. Python follows SQL in demand with notable fluctuations but remained consistently high throughout the year.

3. Tools like Tableau and Power BI showed a declining trend over the year, indicating possible saturation or shifting preferences.

4. Skill demand fluctuated significantly between months, especially around mid-year and Q4, possibly due to hiring cycles or project timelines.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I only got jobs in Germany and looked at their median salary. But first I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs are paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](4_Salary_Analysis.ipynb).

#### Visualize Data 

```python
sns.boxplot(data=df_Germany_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()

```

#### Results

![Salary Distributions of Data Jobs in Germany](SalaryDistribution.png)  
*Box plot visualizing the salary distributions for the top 6 data job titles.*

#### Insights

1. Role-Wise Salary Variation: The chart shows that Machine Learning Engineers and Senior Data Scientists generally have the highest salary ranges, while Data Analysts tend to earn the least among the data roles in Germany.

2. Wider Salary Range for Certain Roles: Machine Learning Engineers and Data Scientists show a broader salary range, indicating higher variability based on experience, skills, or company size.

3. Median Salary Insights: The median salary (the line inside the box) for Senior Data Engineers and Machine Learning Engineers appears significantly higher than for Data Analysts and Data Engineers, highlighting the value of advanced skills and experience.

4. Presence of Outliers: Several roles, such as Senior Data Scientist and Data Analyst, show outliers — extremely high or low salaries  which suggest some positions offer exceptionally high compensation or are affected by niche market factors.

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
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in Germany:

![The Highest Paid & Most In-Demand Skills for Data Analysts in Germany](HighestPaidJob.png)
*Two separate bar graphs visualizing the highest paid skills and most in-demand skills for data analysts in Germany.*

#### Insights:

1. GitHub, BigQuery, and NoSQL are associated with the highest median salaries for data analysts in Germany, each exceeding $150K.

2. Tools like Kafka, Redshift, Terraform, and GCP are highly paid, indicating a premium for cloud and infrastructure-related expertise.

3. While skills like Python, Spark, and Pandas are most in demand (bottom chart), they offer comparatively lower salaries (~$100K), reflecting oversupply.

4. Power BI and Looker are in demand but show lower salary ranges, making them suitable for entry or mid-level professionals.

## 4. What are the most optimal skills to learn for Data Analysts?

To identify the most optimal skills to learn ( the ones that are the highest paid and highest in demand) I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_Optimal_Skills](5_Optimal_Skills.ipynb).

#### Visualize Data

```python
from adjustText import adjust_text
import matplotlib.pyplot as plt

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.show()

```

#### Results

![Most Optimal Skills for Data Analysts in Germany](MostOptimalSkill.png)    
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in Germany.*

#### Insights:

1. GitHub and GCP offer top-tier salaries ($160K–$200K) but appear in fewer job listings, signaling high reward for niche expertise.

2. Python and SQL are highly prevalent in job postings (>35%) with good salaries (~$110K), making them essential core skills.

3. Go and Looker show low demand and relatively low salary, suggesting lower career ROI for data analysts.

4. Technologies like AWS, Azure, and GCP command higher salaries even with moderate job availability, showcasing the value of cloud proficiency.

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

![Most Optimal Skills for Data Analysts in Germany with Coloring by Technology](MostOptimalSkillwithSeaborn.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in Germany with color labels for technology.*

#### Insights:

- The scatter plot shows that most of the `programming` skills (colored blue) tend to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary benefits within the data analytics field.

- The database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry.

- Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries, showing that visualization and data analysis software are crucial for current data roles. This category not only has good salaries but is also versatile across different types of data tasks.

# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market has been incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I got enhance my understanding and provide actionable guidance for anyone looking to advance their career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project is a good foundation for future explorations and underscores the importance of continuous learning and adaptation in the data field.


