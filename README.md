# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most in demand data skills for the top 3 jobs, I filtered out positions based on which were most popular and got the top 5 skills for each of these 3 roles. This query highlights the most popular job titles and their skills, highlighting which skills i should focus on depending on the role I am targeting.

My notebook with detailed steps can be viewed here:
[2_skills_demand.ipynb](3_project\2_skills_demand.ipynb)


## Visualize Data

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