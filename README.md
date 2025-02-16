
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Data Analytics Projects - Sydni Liotta, RDN</title>
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
      margin-bottom: 40px;
    }
    h1, h2, h3, h4 {
      color: #333;
    }
    pre {
      background: #f4f4f4;
      color: #000;
      padding: 15px;
      border-radius: 5px;
      overflow-x: auto;
    }
    code {
      font-family: Consolas, Monaco, 'Andale Mono', 'Ubuntu Mono', monospace;
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
      border-radius: 5px;
      transition: background 0.3s ease;
    }
    .button:hover {
      background-color: #0056b3;
    }
    .image-container img {
      max-width: 100%;
      height: auto;
      border-radius: 10px;
      box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
    }
  </style>
</head>
<body>

<div class="container">
  <h1>Data Analytics Projects</h1>
  <h2>Sydni Liotta, RDN</h2>
  <p><a href="/portfolio/" class="button">Back to Home</a></p>
</div>

<div class="container">
  <h2>Project #1: Flu Shots Dashboard</h2>
  <h3>Question: How can we analyze flu shot compliance rates across different patient demographics and geographic regions to improve outreach efforts?</h3>
  <p><a href="https://public.tableau.com/views/ImmunizationFluShot2023/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link" target="_blank" class="button">View Tableau Dashboard</a></p>
  <div class="image-container">
    <img src="https://raw.githubusercontent.com/sydniliotta/portfolio/main/images/Flu%20Shot%20Analysis%202023.png" alt="Flu Shot Analysis 2023">
  </div>
  <h3>Insights:</h3>
  <ul>
    <li>The biggest jumps in vaccinations occurred between March and April, suggesting an effective campaign or increased patient engagement during this time.</li>
    <li>The 65+ age group (28%) has a surprisingly low rate despite being a high-risk population. This could indicate barriers such as accessibility, awareness, or vaccine hesitancy.</li>
    <li>Targeted interventions may be needed in lower-performing counties like Hampden (38%), Nantucket (38%), and Suffolk (37%).</li>
  </ul>
  <h3>Process:</h3>
  <p>I used SQL to extract anonymized, synthetic patient data on flu vaccinations. Using Tableau, I built interactive visualizations to uncover key trends, including vaccination timing, patient demographics, and overall distribution.</p>
  <h2>SQL Code:</h2>
  <pre><code>
WITH flushot2023 AS (
    SELECT 
        patient, 
        MIN(date) AS firstflushot2023
    FROM public.immunizations
    WHERE code = '5302'
      AND date BETWEEN '2023-01-01' AND '2023-12-31'
    GROUP BY patient
),
activepts AS (
    SELECT DISTINCT patient
    FROM encounters AS e
    JOIN patients AS pat ON e.patient = pat.id
    WHERE start BETWEEN '2023-01-01' AND '2023-12-31'
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

<h2>Tableau Steps</h2>

<h4>1. Prepare Data in Tableau</h4>
<ul>
    <li>Import the dataset containing patient flu shot records.</li>
</ul>

<h4>2. Create Age Group Buckets</h4>
<p>Create a calculated field named <strong>Age Group</strong> with the following formula:</p>
<pre><code>
    IF [Age] >= 0 AND [Age] <= 17 THEN '0-17'
    ELSEIF [Age] >= 18 AND [Age] <= 33 THEN '18-33'
    ELSEIF [Age] >= 34 AND [Age] <= 50 THEN '34-50'
    ELSEIF [Age] >= 51 AND [Age] <= 65 THEN '51-65'
    ELSE '65+'
    END
</code></pre>

<h4>3. Calculate Flu Shot Percentage</h4>
<p>Create a calculated field called <strong>Flu Shot Indicator</strong>:</p>
<pre><code>
    IF [Flu Shot] = 1 THEN 1 ELSE 0 END
</code></pre>

<h4>4. Show Total Compliance and Total Flu Shots Given</h4>
<p>Create two key metrics using FIXED LOD Calculation:</p>
<p><strong>Total Patients in Dataset:</strong></p>
<pre><code>
    { FIXED : SUM([# of Records]) }
</code></pre>
<p><strong>Total Flu Shots Given:</strong></p>
<pre><code>
    SUM([Flu Shot Indicator])
</code></pre>

<h4>5. Create a Running Total of Flu Shots Given</h4>
<ul>
    <li>Drag <strong>Flu Shot Indicator</strong> to <strong>Columns</strong>.</li>
    <li>Drag <strong>Date</strong> (flu shot administration date) to <strong>Rows</strong>.</li>
    <li>Convert into a <strong>Line Chart</strong>.</li>
</ul>

</div>

<div class="container">
  <h2>Project #2: Analyzing Cause of Death Data - Worldwide</h2>
  <h3>Question: What are the primary causes of death that contribute the most to overall mortality, and how can this insight guide public health priorities?</h3>
  <p><a href="https://public.tableau.com/views/TopCausesofDeath-Pareto/TopCausesofDeath-Pareto" target="_blank" class="button">View Tableau Dashboard</a></p>
  <div class="image-container">
    <img src="https://raw.githubusercontent.com/sydniliotta/portfolio/main/images/Cause%20of%20Death%20Pareto.png" alt="Cause of Death Pareto Chart">
  </div>
  <h3>Insights:</h3>
  <ul>
    <li>The leading causes of death account for the majority of mortality, helping public health officials prioritize interventions.</li>
    <li>Applying the Pareto principle (80/20 rule) helps target resources effectively.</li>
    <li>The dynamic parameter allows adjusting the threshold to visualize different cutoffs.</li>
  </ul>
  <h3>Process:</h3>
  <h4>1. Data Preparation:</h4>
  <ul>
    <li>Filtered dataset to include 2015–2019 U.S. data.</li>
    <li>Pivoted death columns for easier analysis.</li>
    <li>Standardized "Causes of Death" using:
      <pre><code>
REPLACE(REPLACE([Reason for Death],'Deaths -',''),'- Sex: Both - Age: All Ages (Number)','')
      </code></pre>
    </li>
  </ul>
  <h4>2. Building the Pareto Chart:</h4>
  <ul>
    <li>Created a dual-axis visualization using Deaths and a running total.</li>
    <li>Converted the second axis into a percentage cumulative line graph.</li>
    <li>Filtered to show the top causes contributing to 80% of total deaths.</li>
  </ul>
  <p><a href="/portfolio/" class="button">Back to Home</a></p>
</div>

</body>
</html>
