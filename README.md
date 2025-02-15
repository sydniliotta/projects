## Sydni Liotta, RDN

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Projects</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f8f9fa;
            margin: 40px;
            padding: 0;
            text-align: center;
        }
        h1 {
            font-size: 36px;
            color: #333;
            margin-bottom: 20px;
        }
        .container {
            max-width: 900px;
            margin: auto;
            background: white;
            padding: 20px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
            text-align: left;
        }
        h2, h3, h4 {
            color: #333;
        }
        ul {
            padding-left: 20px;
        }
        ul li {
            margin: 8px 0;
        }
        a {
            text-decoration: none;
            color: #007bff;
            font-weight: bold;
        }
        .button {
            display: inline-block;
            margin-top: 20px;
            padding: 12px 24px;
            font-size: 16px;
            font-weight: bold;
            color: white;
            background-color: #007bff;
            text-decoration: none;
            border-radius: 5px;
            transition: background 0.3s ease;
        }
        .button:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
<h1>Projects</h1> <!-- Moved outside the box -->
<div class="container">
    <h2>Project #1: Analyzing Cause of Death Data - Worldwide</h2>
    <h3>Question: What are the primary causes of death that contribute the most to overall mortality, and how can this insight guide public health priorities?</h3>
    
    <p><a href="https://public.tableau.com/views/TopCausesofDeath-Pareto/TopCausesofDeath-Pareto" target="_blank" class="button">View Tableau Dashboard</a></p>
    
<img src="https://raw.githubusercontent.com/sydniliotta/portfolio/main/images/Cause%20of%20Death%20Pareto.png" alt="Cause of Death Pareto Chart" style="max-width:100%; height:auto; border-radius: 10px; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);">
    <h3>Process</h3>
    
    <h4>1. Data Preparation:</h4>
    <ul>
        <li>Imported the dataset and filtered it to only include 2015–2019 data for the U.S.</li>
        <li>Pivoted the death columns for easier analysis.</li>
        <li>Cleaned and standardized the Causes of Death field using:
            <code>REPLACE(REPLACE([Reason for Death],'Deaths -',''),'- Sex: Both - Age: All Ages (Number)','')</code>
            <div class="image-container">
        <img src="https://github.com/sydniliotta/portfolio/blob/main/images/cause%20of%20death%20data%20cleaning.png?raw=true" alt="Cleaning Data">
    </div>
        </li>
    </ul>
    <h4>2. Building the Pareto Chart:</h4>
    <ul>
        <li>Duplicated the Deaths measure to create a dual-axis visualization.</li>
        <li>Converted the second axis into a running total to calculate the cumulative percentage of deaths.</li>
        <li>Changed the cumulative axis to a line graph and formatted it as a percentage of total deaths.</li>
        <li>Applied a secondary calculation (percent of total) to visualize cumulative impact.</li>
    </ul>
    <h4>3. Applying the 80/20 Rule:</h4>
    <ul>
        <li>Created a parameter (80%) to dynamically adjust the threshold.</li>
        <li>Developed a calculated field to filter only the causes contributing to ≤ 80% of total deaths.</li>
        <li>Filtered the view to only display the top 80% of causes, making the visualization more focused and impactful.</li>
    </ul>
    <h3>Insights:</h3>
    <ul>
        <li>This visualization highlights the few leading causes of death that account for the majority of mortality, helping public health officials prioritize interventions.</li>
        <li>The dynamic parameter allows for adjusting the percentage threshold, demonstrating how different cutoffs impact the analysis.</li>
        <li>By applying the Pareto principle, we can target resources where they will have the greatest impact on reducing mortality. If we aim to reduce deaths most effectively, this is where we should start.</li>
    </ul>
    <p><a href="/portfolio/" class="button">Back to Home</a></p>
</div>
</body>
</html>

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Flu Shots Dashboard Project</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #f8f9fa;
      margin: 40px;
      line-height: 1.6;
      color: #333;
    }
    .container {
      max-width: 900px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    h1, h2 {
      color: #333;
    }
    pre {
      background: #f4f4f4; /* Light background */
      color: #000;       /* Black text */
      padding: 15px;
      border-radius: 5px;
      overflow-x: auto;
    }
    code {
      font-family: Consolas, Monaco, 'Andale Mono', 'Ubuntu Mono', monospace;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Project # 1 Flu Shots Dashboard</h2>
       <h3>Question: What key trends and patient demographics can we uncover from our 2023 flu shot data, and how can these insights inform strategies to improve vaccination outreach?</h3>
    
    <p><a href="https://public.tableau.com/views/ImmunizationFluShot2023/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link" target="_blank" class="button">View Tableau Dashboard</a></p>
    <p>
      In this project, I analyzed flu shot data from a hospital system for the year 2023. I used SQL to extract anonymized, synthetic data on patients who received the flu vaccine (this is not real patient data) and then cleaned and prepared the dataset for analysis.
      Leveraging Tableau, I built interactive visualizations to explore key trends, such as vaccination timing, patient demographics, and overall vaccination distribution. These dashboards provide actionable insights that could support public health decision-making and 
      strategic planning.
       This project highlights my proficiency in SQL for data extraction and Tableau for data visualization, as well as my commitment to ethical data handling in healthcare analytics.
        
    </p>
    <h2>SQL Code</h2>
    <pre><code>
/* Flu Shots Dashboard 
   Build a dashboard to track flu shots in 2023 with the following metrics:
     1. Total % of patients getting flu shots, stratified by:
          - Age
          - Race
          - County (on a map)
          - Overall
     2. Running total of flu shots over 2023
     3. Total number of flu shots given in 2023
     4. A list of patients indicating whether they received a flu shot
   Requirement:
     - Include only patients active in our hospital.
   Note:
     - Patients may have multiple flu shots; we use a CTE to create a one-to-one join by selecting the earliest flu shot per patient.
*/

WITH flushot2023 AS (
    SELECT 
        patient, 
        MIN(date) AS firstflushot2023
    FROM public.immunizations
    WHERE code = '5302'
      AND date BETWEEN '2023-01-01 00:00' AND '2023-12-31 23:59'
    GROUP BY patient
),
activepts AS (
    SELECT DISTINCT patient
    FROM encounters AS e
    JOIN patients AS pat ON e.patient = pat.id
    WHERE start BETWEEN '2023-01-01 00:00' AND '2023-12-31 23:59'
      AND pat.deathdate IS NULL
      AND EXTRACT(MONTH FROM age('2023-12-31', pat.birthdate)) >= 6
)
SELECT 
    pat.id,
    pat.birthdate,
    pat.race,
    pat.county,
    pat.first,
    pat.last,
    EXTRACT(YEAR FROM age('2023-12-31', pat.birthdate)) AS age,
    flushot.firstflushot2023,
    CASE 
        WHEN flushot.patient IS NOT NULL THEN 1 
        ELSE 0 
    END AS flushot2023
FROM public.patients AS pat
LEFT JOIN flushot2023 AS flushot ON pat.id = flushot.patient
WHERE pat.id IN (SELECT patient FROM activepts);
    </code></pre>
    <p>
      This query forms the basis of the Tableau dashboard, which visualizes key metrics such as patient demographics, running totals, and overall counts of flu shots administered in 2023.
    </p>
  </div>
</body>
</html>
