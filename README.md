# The Analysis

## 1. What are the most demanded skills for the top 3 most popular Data roles?

To find the most in demand data skills for the top 3 jobs, I filtered out positions based on which were most popular and got the top 5 skills for each of these 3 roles. This query highlights the most popular job titles and their skills, highlighting which skills i should focus on depending on the role I am targeting.

My notebook with detailed steps can be viewed here:
[2_skills_demand.ipynb](3_project\2_skills_demand.ipynb)


## Creating the Visualizations:

```fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_count[df_skills_count['job_title_short'] == job_title].head(5)
    df_plot.plot(kind='barh', x='job_skills', y='skill_count', ax=ax[i], title=job_title)
    ax[i].invert_yaxis()
    ax[i].set_ylabel('')
    ax[i].legend().set_visible(False)
    
fig.suptitle('Counts of Top Skills in Job Postings', fontsize=15)
fig.tight_layout()
plt.show()
```
```
fig, ax = plt.subplots(len(job_titles), 1)

sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].legend().set_visible(False)
    ax[i].set_xlim(0, 68)
    
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va = 'center')
        
    if i != len(job_titles) -1:
        ax[i].set_xticks([])
    
fig.suptitle('Probability of Skills Requested in Job Postings for Data Jobs in Sweden', fontsize=15)
fig.tight_layout()
plt.show()
```


## Results

![Visualization of Top Skills for Data Jobs](3_project\Images\job_postings_count.png)

![Probability of Skill in Job Ad for Different Roles](3_project\Images\job_probability.png)

## Insights

- Python and SQL are versatile and in-demand skill across all 3 of the top jobs. Python is the top skill for Data Engineers (61% )and Data Scientists (62%).
- SQL is the most requested skill for Data Analysts with 58% probability it will be requested for Data Analysts and 49% for Data Scientists. For Data Engineers Python is the most requested skill and the chance of it being requested in a job ad is 61%.
- Data Engineers require a more cloud focussed technical skillset (Azure, AWS, GCP) compared to Data Analysts who are expected to be more efficient in more general data mangement and analysis tools like Tableau and Power BI.

## 2. How are in-demand skills trending for Data Analysts?


To see how trends are developing over the course of a year, I first separated the skills out from their job listing and created a new dataframe that ordered the count of job skills and their percentagage chance of appearing in job ads

## Creating the Data-Frame:

```Python
df_da_swe_pivot = df_da_swe_explode.pivot_table(index='job_posted_month_no', columns='job_skills', aggfunc='size', fill_value=0)
df_da_swe_pivot.loc['Total']  = df_da_swe_pivot.sum()
df_da_swe_pivot = df_da_swe_pivot[df_da_swe_pivot.loc['Total'].sort_values(ascending=False).index]
df_da_swe_pivot = df_da_swe_pivot.drop('Total')
df_swe = df[df['job_country'] == 'Sweden']

df_da_swe['job_posted_month_no'] = df_da_swe['job_posted_date'].dt.month
df_da_swe_totals = df_da_swe.groupby('job_posted_month_no').size()
df_da_swe_percent = df_da_swe_pivot.div(df_da_swe_totals/100, axis=0)
df_da_swe_percent = df_da_swe_percent.reset_index()
df_da_swe_percent['job_posted_month'] = df_da_swe_percent['job_posted_month_no'].apply(lambda x: pd.to_datetime(x, format= '%m').strftime('%b'))
df_da_swe_percent = df_da_swe_percent.set_index('job_posted_month')
df_da_swe_percent = df_da_swe_percent.drop(columns = 'job_posted_month_no')
```

## Visualize Data

![Trending Skills](3_project\Images\skills_trend.png)*Illustrating how top skills demand change over the course of a year*

## Insights

- SQL remains the top demanded skill throughout the year although there is a drop in demand in the final three months.
- Tableau experiences a gradual growth throughout the year with a sharp drop-off in september through to the end of the year.
- Python shows stable demand with some fluctuations but remains an essential skill for Data Analysts. Power BI, while less demanded shows a gradual trend in demand. 


## 3. How well do jobs and skills pay for Data Analysts?

## Creating the Data-Frame:

´´´
## Visualize Data

![Salary Distribution Analysis](3_project\Images\salary_distribution.png)*Illustrating how salary is distributed among top data jobs*

## Insights

- There is significant variation in salary between the top 6 jobs. Senior positions tend to have the highest salary potential with over 2.5 M sek indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data engineer and Data Science roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills can lead to high pay in these roles. Conversely, Data Analyst and Business Analysts show fewer outliers, reflecting more consistency in salary.

- Median salaries increase with seniority and specialization of the roles. Senior roles not only have higher median salaries bu also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

## 4. How well do jobs and skills pay for Data

## Visualize Data

![In Demand Job Skills](3_project\Images\in_demand_skills.png)
*Highlighting which skills are in demand and their relative pay*

## Insights

- Tools like `Excel`,``Hadoop, Word, VBA`` are among the highest paid but are not in strong demand. High pay does not equal high demand.

- ``Power BI`` and ``SQL`` are both in high pay and high demand suggesting these are valuable skills to focus on.

- Skills like ``Python``, ``Azure`, `Databricks`` are in demand but slightly lower paid reflecting growing adoption and more mid level roles.