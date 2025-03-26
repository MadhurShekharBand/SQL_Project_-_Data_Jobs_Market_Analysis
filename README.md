# Introduction:
Focusing on data analyst roles, this project explores top-paying jobs, in-demand skills, and where does high demand meets high salary in data analytics sector.

# Project Files:
- **SQL Queries:** All SQL queries are available here: [SQL Queries](https://github.com/MadhurShekharBand/SQL_Project_-_Data_Jobs_Market_Analysis/blob/de96ab3fb9dc17445a7359bc0ba266ee7c3b981a/SQL%20Query%20File%20-%20Microsoft%20SQL%20Server.sql)
- **Data Files:** The data files in Microsoft Excel File format is available here: [Data Files](https://github.com/MadhurShekharBand/SQL_Project_-_Data_Jobs_Market_Analysis/tree/de96ab3fb9dc17445a7359bc0ba266ee7c3b981a/Data) 
- **Outputs of All SQL Queries:** Outputs of all SQL queries from Microsoft SQL Server are available here in Microsoft Excel Comma Separated Values File (.csv) format: [Outputs of All SQL Queries](https://github.com/MadhurShekharBand/SQL_Project_-_Data_Jobs_Market_Analysis/tree/de96ab3fb9dc17445a7359bc0ba266ee7c3b981a/SQL%20Queries%20Outcomes)
- **Visualizations of the SQL Queries:** A Microsoft Power BI file containing visualizations of all SQL queries is available here: [Visualizations of the SQL Queries](https://github.com/MadhurShekharBand/SQL_Project_-_Data_Jobs_Market_Analysis/tree/de96ab3fb9dc17445a7359bc0ba266ee7c3b981a/SQL%20Queries%20Visualizations)

# Background:

### The questions I wanted to answer through my SQL queries were:
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

### About the Dataset:
The dataset used for this project includes real-world job data sourced from the field of data science, specifically focusing on salaries across various job titles, countries, and types of work. It includes key variables such as job titles, salaries, locations, and required skills, all of which are crucial for understanding salary trends and making informed career decisions. The dataset reflects salaries in different countries and regions, with a focus on providing median salary values, which offer a more stable and representative measure of typical earnings within each job category.

### Tools I Used:
- **Microsoft SQL Server:** The chosen database management system, ideal for handling the job posting data and executing SQL queries.
- **Microsoft Power BI:** The chosen BI tool used to visualize my SQL query outputs.

# 1. What are the top-paying data analyst jobs?
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

```sql
SELECT TOP 10
	job_id,
	job_title,
	job_location,
	job_schedule_type,
	salary_year_avg,
	job_posted_date,
	name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND 
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC;
```

### Insights:
I made below bar graph from my SQL query output using Power BI.

![Query 1 Visualization Screenshot](https://github.com/user-attachments/assets/5b7af1bb-b77e-40f3-95f5-1eb351feedc7)

The top data analyst jobs in 2023 offer significant salary potential, with the top 10 paying roles ranging from $184,000 to $650,000, highlighting the lucrative opportunities in the field. A variety of companies, including SmartAsset, Meta, and AT&T, are offering these high-paying positions, demonstrating strong interest across different industries. Additionally, there is a wide range of job titles, from Data Analyst to Director of Analytics, showcasing the diversity of roles and specializations within the data analytics field.

# 2. What skills are required for these top-paying jobs?
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```sql
WITH top_paying_jobs AS (
    SELECT TOP 10
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND 
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

### Insights:
I made below bar graph from my SQL query output using Power BI.

![Query 2 Visualization Screenshot](https://github.com/user-attachments/assets/403908c4-89dd-40e0-9d6a-d9b52fb9ac8a)

The most demanded skills for the top 10 highest paying data analyst jobs in 2023 include SQL, which is required in 8 of the roles, followed closely by Python in 7 positions. Tableau is also highly sought after, appearing in 6 of the job listings. Other skills such as R, Snowflake, Pandas, and Excel are also in demand, though to varying degrees.

# 3. What skills are most in demand for data analysts?
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT TOP 5
	skills,
	COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
	job_title_short = 'Data Analyst' 
	AND job_work_from_home = 1 
GROUP BY
	skills
ORDER BY
	demand_count DESC;
```

### Insights:
I made below bar graph from my SQL query output using Power BI.

![Query 3 Visualization Screenshot](https://github.com/user-attachments/assets/9a5fee8e-5352-4772-9b37-1c468e0120c4)

The most demanded skills for data analysts in 2023 highlight the importance of both foundational and technical expertise. SQL and Excel remain fundamental, emphasizing the need for strong skills in data processing and spreadsheet manipulation. Meanwhile, programming and visualization tools like Python, Tableau, and Power BI are essential, reflecting the growing significance of technical skills in data storytelling and decision support.

# 4. Which skills are associated with higher salaries?
Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT TOP 25
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC;
```

### Insights:
I made below bar graph from my SQL query output using Power BI.

![Query 4 Visualization Screenshot](https://github.com/user-attachments/assets/5d0afda1-eeb3-4ebe-b41b-268c2bef81e7)

The top-paying skills for data analysts in 2023 highlight the high value placed on advanced technical expertise. Analysts skilled in big data technologies like PySpark and Couchbase, machine learning tools such as DataRobot and Jupyter, and Python libraries like Pandas and NumPy command the highest salaries, reflecting the industry's emphasis on data processing and predictive modeling. Proficiency in software development and deployment tools like GitLab, Kubernetes, and Airflow indicates a lucrative intersection between data analysis and engineering, with a premium placed on automation and efficient data pipeline management. Additionally, expertise in cloud computing and data engineering tools such as Elasticsearch, Databricks, and GCP underscores the growing importance of cloud-based analytics, suggesting that cloud proficiency significantly boosts earning potential in the field.

# 5. What are the most optimal skills to learn?
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```sql
SELECT TOP 25
	skills_dim.skill_id,
	skills_dim.skills,
	COUNT(skills_job_dim.job_id) AS demand_count,
	ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
	job_title_short = 'Data Analyst'
	AND salary_year_avg IS NOT NULL
	AND job_work_from_home = 1 
GROUP BY
	skills_dim.skill_id,
	skills_dim.skills
HAVING
	COUNT(skills_job_dim.job_id) > 10
ORDER BY
	avg_salary DESC,
	demand_count DESC;
```

### Insights:
I made below table from my SQL query output using Power BI.

![Query 5 Visualization Screenshot](https://github.com/user-attachments/assets/19dacb20-ee40-422b-9677-6c2bfd0913de)

- In 2023, the most optimal skills for data analysts reflect a blend of programming, cloud technologies, business intelligence, and database management. Python and R stand out as high-demand programming languages, with demand counts of 236 and 148 respectively, though their average salaries—around $101,397 for Python and $100,499 for R—indicate that while proficiency in these languages is highly valued, they are also widely available. 
- Skills in cloud tools such as Snowflake, Azure, AWS, and BigQuery are in significant demand, with relatively high average salaries, highlighting the increasing importance of cloud platforms and big data technologies.
- Business intelligence and visualization tools like Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, emphasize the growing role of data visualization in deriving actionable insights.
- Finally, demand for expertise in traditional and NoSQL databases (Oracle, SQL Server, NoSQL), with average salaries ranging from $97,786 to $104,534, reflects the continued need for data storage, retrieval, and management skills.

# Conclusion:
### What I Learned:

Throughout this journey, I have significantly enhanced my SQL skillset with advanced techniques. I have mastered complex query crafting, including efficiently merging tables and utilizing WITH clauses for creating temporary tables. I have also gained proficiency in data aggregation, leveraging GROUP BY and aggregate functions such as COUNT() and AVG() to effectively summarize data. Additionally, I have refined my analytical abilities, transforming complex questions into actionable, insightful SQL queries that drive data-driven decision-making.

### Closing Thoughts:

This project not only enhanced my SQL skills but also provided valuable insights into the data analyst job market. The findings from the analysis offer a clear guide for prioritizing skill development and job search strategies. By focusing on high-demand, high-salary skills, aspiring data analysts can better position themselves in a competitive job market. This exploration underscores the importance of continuous learning and adaptability to emerging trends within the data analytics field.
