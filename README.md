# projects

---
layout: default
title: Projects
permalink: /projects/
---

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
