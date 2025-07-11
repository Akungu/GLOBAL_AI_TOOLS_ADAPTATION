
# Overview
I carried out this study so as to analyze globalal AI adoptation trends in the world, from 2023 to 2024.

# Background
The source of the dataset is [ Kaggle ]('https://www.kaggle.com/datasets/tfisthis/global-ai-tool-adoption-across-industries'). It contains 145,000 rows with the following 9 columns:
- country
- industry
- ai_tool
- adoption_rate
- daily_active_users
- year
- user_feedback
- age_group
- company_size

### The questions I wanted to answer  were :
1. Which industries and regions are leading in in AI tool adoption?

2. How do company size and user age group affect AI integration?

3. What are the most common sentiments and topics in user feedback?

4. How has AI tool adoption changed from 2023 to 2024?

5. Which AI tools are most popular and why?

# Tools I used
- **Python**: The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
  - **Pandas Library**: This was used to analyze the data.
  - **Matplotlib Library**: I used it to visualize the data.
  - **Seaborn Library**: Helped me create more advanced visuals.
  - **Jupyter Notebooks**: The tool I used to run my Python scripts which let me easily include my notes and analysis.
  - **Visual Studio Code**: My go-to for executing my Python scripts.
  - **Git & GitHub**: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Cleaning
## Import and Clean the Data
```python
# import libraries
import pandas as pd
import matplotlib.pyplot as plt
import ast
from datasets import load_dataset
import seaborn as sns

# reading the  dataset
df = pd.read_csv('ai_adoption_dataset.csv')
print(df.head())
print(df.info())
```
NB: The data was already in it's clean form therefore there was no need for data cleaning.

# Analysis
### 1. Which industries and regions are leading  in AI tool adoption?

```python
df_leading_industries = df['industry'].value_counts().head(10).reset_index(name='Count')
df_leading_regions = df['country'].value_counts().head(10).reset_index(name='Count')
df_leading_regions

fig, ax = plt.subplots(2,1, figsize=(8,14))

sns.barplot(x='industry',y='Count',data=df_leading_industries, ax=ax[0], palette='pastel', hue='Count' )
ax[0].set_title('Industries in AI Adaptation')
ax[0].set_xlabel('')
ax[0].set_ylabel('')
ax[0].tick_params(axis='x' , rotation=45)
ax[0].tick_params(labelsize=8)
ax[0].get_legend().set_visible(False)

sns.barplot( x='country',y='Count',data=df_leading_regions, ax=ax[1], palette='pastel', hue='Count' )
ax[1].set_title('Leading Regions in AI Adaptation')
ax[1].set_xlabel('')
ax[1].set_ylabel('')
ax[1].tick_params(axis='x' , rotation=45)
ax[1].tick_params(labelsize=8)
ax[1].get_legend().set_visible(False)

plt.show()
plt.tight_layout()

```

### Results
![leading](/assets/leading_adoption.png)

### Insights
- There is an almost even distribution in the total counts of industries that lead in AI adoption. 
- Manufacturing indusry leads and is followed closely by others such as education, technology, healthcare, and agriculture.
- All the major important industries in these given regions rely heavily on AI technology.
- The Western countries and India are far ahead in terms of ai adoption, with Austrslia leading.

### 2. How do company size and user age group affect AI integration?
```python
df_user_age_group = df_age_group.groupby('age_group')['adoption_rate'].agg('mean').reset_index(name='Adoption Rate').sort_values(by='Adoption Rate', ascending=False)
df_user_age_group


ax =sns.barplot(data=df_user_age_group, x='Adoption Rate', y='age_group', hue='Adoption Rate', palette='dark:b_r')
ax.get_legend().set_visible(False)
ax.set_title('Effects of Age Group in AI Intergration')
ax.set_ylabel('')
```

```python
df_company_size_final = df_company_size.groupby('company_size')['adoption_rate'].agg('mean').reset_index(name='Adoption Rate')
df_company_size_final_1 = df_company_size_final.sort_values(by='Adoption Rate', ascending=False)
df_company_size_final_1

labels = ['Startup', 'Enterprises','SME']
sizes=[50.037439, 49.836855, 49.743258]
colors=['#00008B','#4169E1','#ADD8E6']

plt.figure(figsize=(4,4))
plt.pie(sizes, labels=labels, autopct='%1.1f%%',startangle=180, colors=colors)
plt.title('Effects of Company Size on AI Adaptation')
plt.axis('equal')
plt.show()
```
### Results
![age_group](/assets/age_group.png)

![company_size](/assets/company_size.png)

### Insights
- Users the age of 55 and above have the highest AI adotion rate, while the 25-34 have the least.
- There are no significant differences in the adoption rate between the age groups.
- For companies, Startups lead in AI adoption rate. Enterprises and SME's follow closely with a difference of .1% among the three.

### 3. What are the most common sentiments and topics in user feedback?
The feedback in the study contains data that is not in the English language and therefore returns a feedback sentiment of 0.0, rendering it inconclusive.

### 4. How has AI tool adoption changed from 2023 to 2024?
```python
df_trend = df.copy()
df_adoptation_trend = df_trend.pivot_table(
    values='daily_active_users',
    index='ai_tool',
    columns='year',
    aggfunc='count'

)

ax =df_adoptation_trend.plot(kind='bar')
ax.set_title('AI Adoptation Trend')
ax.set_ylabel('Daily_active_users')
ax.tick_params(axis='x' , rotation=45)
```
### Results
![trend](/assets/trend.png)

### Insights
- There was an almost 50% growth in the usage of all the AI tools from 2023 to 2024.
- ChatGPT was the most popular tool through out the 2 years.

```python
df_adaptation_rate = df.copy()
df_daily_users = df.copy()
fig, ax = plt.subplots(1,2)
plt.figure(figsize=(12,20))
sns.boxplot(data=df_adaptation_rate, x='adoption_rate', vert=False , ax=ax[0])
ax[0].set_title('Adoptation Rate')
ax[0].set_xlabel('')
sns.boxplot(data=df_daily_users, x='daily_active_users', vert=False , ax=ax[1])
ax[1].set_title('Daily Active Users')
ax[1].set_xlabel('')
ax[1].set_xlabel('')
```
### Results
![adotion_rate](/assets/adotion_rate.png)

### Insights
- There were no outliers for both the adoption rate and daily active users distribution.


### 5. Which AI tools are most popular and why?
```python
df_tools = df.copy()
df_ai_tools = df_tools.groupby('ai_tool')['daily_active_users'].agg('count').reset_index(name='Count').sort_values(by='Count', ascending=False).head(3)
df_ai_tools

labels = ['ChatGPT', 'Midjourney','Stable Diffusion']
sizes=[58045, 43324, 21826]
colors=['#00008B','#4169E1','#ADD8E6']

plt.figure(figsize=(4,4))
plt.pie(sizes, labels=labels, autopct='%1.1f%%',startangle=165, colors=colors)
plt.title('Most Popular AI Tools')
plt.axis('equal')
plt.show()
```
### Results
![popular_tools](/assets/popular_tools.png)

### Insights
- ChatGPT is by far the most popular AI tool as it has the highest adoption rate percentage of 47.1%. This can be attributed to the fact that it is free to core functions.
- Midjourney and Stable Diffusion are the other popular AI tools after ChatGPT.

# Challanges
The user_feedback was not in the English Language therefore I was unable to draw valid conclusions from them.

# Lessons Learnt
I learnt how to gain insights into user feedback using sentiment analysis.






