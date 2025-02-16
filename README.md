# Data Analytics Projects 
## Sydni Liotta, RDN


    <p><a href="/portfolio/" class="button">Back to Home</a></p>

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
       <h3>Question: How can we analyze flu shot compliance rates across different patient demographics and geographic regions to identify trends and improve vaccination outreach efforts?</h3>
    
    <p><a href="https://public.tableau.com/views/ImmunizationFluShot2023/Dashboard1?:language=en-US&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link" target="_blank" class="button">View Tableau Dashboard</a></p>
    <img src="https://raw.githubusercontent.com/sydniliotta/portfolio/main/images/Flu%20Shot%20Analysis%202023.png" alt="Cause of Death Pareto Chart" style="max-width:100%; height:auto; border-radius: 10px; box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);">
    <p>
      I used SQL to extract anonymized, synthetic patient data on flu vaccinations. Using Tableau, I built interactive visualizations to uncover key trends, including vaccination timing, patient demographics, and overall distribution. Additionally, I provided a county-level breakdown to identify geographic areas with lower vaccination rates, helping to target interventions and improve outreach efforts. These dashboards offer actionable insights to support public health decision-making and strategic planning.
      
    </p>
    <h2>SQL Code</h2>
    <pre><code>
    
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
    </p>

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
        ELSEIF [Age] >= 34 AND [Age] <= 48 THEN '34-48'
        ELSEIF [Age] >= 49 AND [Age] <= 64 THEN '49-64'
        ELSE '65+'
        END
    </code></pre>

    <h4>3. Calculate Flu Shot Percentage</h4>
    <p>Create a calculated field called <strong>Flu Shot Indicator</strong>:</p>
    <pre><code>
      IF [Flu Shot] = 1 THEN 1 ELSE 0 END
    </code></pre>


    <h3>6. Show Total Compliance and Total Flu Shots Given</h3>
    <p>Create two key metrics using FIXED LOD Calculation:</p>

    <p><strong>Total Patients in Dataset:</strong></p>
    <pre><code>
      { FIXED : SUM([# of Records]) }
    </code></pre>

    <p><strong>Total Flu Shots Given:</strong></p>
    <pre><code>
      SUM([Flu Shot Indicator])
    </code></pre>

    <h3>7. Create a Running Total of Flu Shots Given</h3>
    <ul>
        <li>Drag <strong>Flu Shot Indicator</strong> to <strong>Columns</strong>.</li>
        <li>Drag <strong>Date</strong> (flu shot administration date) to <strong>Rows</strong>.</li>
        <li>Convert into a <strong>Running Total</strong>:
            <ul>
                <li>Right-click <strong>Flu Shot Indicator</strong> > <strong>Quick Table Calculation</strong> > <strong>Running Total</strong>.</li>
            </ul>
        </li>
        <li>Convert into a <strong>Line Chart</strong>.</li>
    </ul>

</div>

</body>
</html>

  </div>
</body>
</html>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title></title>
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
<h1></h1> <!-- Moved outside the box -->
<div class="container">
    <h2>Project #2: Analyzing Cause of Death Data - Worldwide</h2>
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
</div>
</body>
  
    <p><a href="/portfolio/" class="button">Back to Home</a></p>

</html>

