# Data Analysis Projects
## Sydni Liotta, RDN

------
layout: default
title: Projects
permalink: /projects/
<!DOCTYPE html>
--------
---
layout: default
title: Projects
permalink: /projects/
---

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
    <h1>Flu Shots Dashboard Project</h1>
    <p>
      This project tracks how many patients received flu shots in 2023. I used SQL to extract and transform data from our hospital system, and then created visualizations in Tableau.
    </p>
    <h2>SQL Code</h2>
    <pre><code>
/* Flu Shots Dashboard - SQL & Tableau
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
