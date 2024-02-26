# Survey Response Analysis

### Table of Contents
- [Project Overview](#project_overview)
- [Data Source](#data-source)
- [Tools/Skills](#tools/Skills)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings/Recommendations](#findings-recommendations)
- [Limitations](limitations)


## Project Overview
The Employee Survey Responses are actual responses from an employee engagement survey conducted by Pierce County WA and completed voluntarily by government employees. The dataset is a Single table and contains 14,725 records. The total number of fields is 10. Thie aim of this analysis is to generate insights to help improve employee job satisfaction

## Data Source
This is the main dataset used in this analysis "Employee Survey - HR Survey Reponse.csv" file, containing detailed information about each survey question and employee response. [Download_here](https://docs.google.com/spreadsheets/d/1nbhfp2ModgqDAPveYQG9CknRw2PYJQxbOTs3xSKOB8E/edit#gid=61186505)

## Tools/Skills
-  Power Query for Data Cleaning
-  SQL Server for Exploratory Data Analysis
-  Power BI for Data Visualization and Reports

## Data Cleaning
1. There are 15 duplicate rows which where cleaned using power query
2. Data Formatting

## Exploratory Data Analysis
1. Which survey question did respondents agree with or disagree with the most?
2. Do yo see any patterns or trends by department or role?
3. Questions Respondenets totally agreed with and disagreed with the most by Job roles
4. Questions Respondenets totally agreed with and disagreed with the most by department
5. As a data analyst, what steps might you take to improve employee satisfaction based on the survey results?

![employee survey response dashboard](employee_survey_response_dashboard.png)

![employee survey response dashboard](https://github.com/BukolaOrire/Employee_Survey_Response/assets/161165047/3977358f-d31e-49a6-90df-5433c2232626)



## Data Analysis
``` sql
 SELECT * FROM Employee_Survey 

 ---survey questions respondants agree and disagree with the most 
 SELECT Question, Response_text
       ,COUNT(Response_text) AS Response_count
 FROM Employee_Survey 
 GROUP BY  Question, Response_text
 HAVING COUNT(Response_text) IN (189,469,663,846)
 ORDER BY 3 DESC

 ---What Department has the Highest number of Response
 SELECT Department,COUNT(Response) AS Respondents 
 FROM Employee_Survey 
 WHERE Status = 'Complete'
 GROUP BY Department
 ORDER BY 2 DESC

 ----What Question did Respondents Agree With by Department ? 
 SELECT Department,Question
       ,SUM(CASE WHEN Response BETWEEN 3 AND 4 THEN 1 ELSE 0 END) AS Totally_Agree
 FROM Employee_Survey 
 WHERE Status ='Complete' 
 GROUP BY Department,Question
 ORDER BY  1, 3 DESC

  ----What Question did Respondents Disagree With by Department ? 
 SELECT Department,Question
	   ,SUM(CASE WHEN Response BETWEEN 1 AND 2 THEN 1 ELSE 0 END ) AS Totally_Disagree
 FROM Employee_Survey 
 WHERE Status ='Complete' 
 GROUP BY Department,Question
 ORDER BY  1, 3 DESC


-- What Survey Questions did Respondents Agree/Disagree with by Job Role
--Survey Questions Directors Agreed and Disagreed with the most 
SELECT Question,
       SUM(CASE WHEN Response BETWEEN 3 AND 4 THEN 1 ELSE 0 END) AS Totally_Agree,
	   SUM(CASE WHEN Response BETWEEN 1 AND 2 THEN 1 ELSE 0 END ) AS Totally_Disagree
FROM Employee_Survey 
WHERE Status = 'Complete' AND  Director = '1'
GROUP BY Question
ORDER BY 1, 2

--Survey Questions Managers Agreed and Disagreed with the most 
SELECT Question,
       SUM(CASE WHEN Response BETWEEN 3 AND 4 THEN 1 ELSE 0 END) AS Totally_Agree,
	   SUM(CASE WHEN Response BETWEEN 1 AND 2 THEN 1 ELSE 0 END ) AS Totally_Disagree
FROM Employee_Survey 
WHERE Status = 'Complete' AND  Manager = '1'
GROUP BY Question
ORDER BY 1, 3 DESC

--Survey Questions Staff Agreed and Disagreed with the most 
SELECT Question,
       SUM(CASE WHEN Response BETWEEN 3 AND 4 THEN 1 ELSE 0 END) AS Totally_Agree,
	   SUM(CASE WHEN Response BETWEEN 1 AND 2 THEN 1 ELSE 0 END ) AS Totally_Disagree
FROM Employee_Survey 
WHERE Status = 'Complete' AND  Staff = '1'
GROUP BY Question
ORDER BY 1, 2

--Survey Questions Surpervisors Agreed and Disagreed with the most 
SELECT  Question,
       SUM(CASE WHEN Response BETWEEN 3 AND 4 THEN 1 ELSE 0 END) AS Totally_Agree,
	   SUM(CASE WHEN Response BETWEEN 1 AND 2 THEN 1 ELSE 0 END ) AS Totally_Disagree
FROM Employee_Survey 
WHERE Status = 'Complete' AND  Supervisor = '1'
GROUP BY Question
ORDER BY 1, 2

---Analysis on Respondents with incomplete status by Job Role
SELECT Department,COUNT(Response_ID) AS Incomplete_Response_Count
FROM Employee_Survey 
WHERE Status = 'Incomplete' 
GROUP BY ROLLUP (Department)
ORDER BY 2 DESC
```
## Findings/Recommendations
- Staff, Managers, and Supervisors have the most disagreeing response to question (3.In the last Seven days , I have received 
  recognition  or praise for doing a good work) This proves they don't feel valued for their work.
- Management need to implement a better performance appraisal method and conduct proper employee performance evaluations to ensure that 
 employees are being recognized for their work. Advancement in employee training and career development and offering adequate reward and 
  other other benefits will help to improve performance and satisfaction. Employee sensitization and training will help foster awareness 
  and eliminate any form of bias in the workplace.
- Generally, Directors are the least dissatisfied. Further analysis revealed job satisfaction is low. Among 1,452 respondents to 
  question 1, a total of 1,107 respondents are dissatisfied with their job.

  ## Limitations
 I did not make use of Column status where status in "Incomplete" for analysis on employees Overall response rating as they are unrealiable
 
