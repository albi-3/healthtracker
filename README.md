<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Health Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4; /* Light gray background */
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh; /* Full viewport height */
        }

        #container {
            background-color: #fff; /* White container background */
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); /* Soft shadow */
            max-width: 400px;
            width: 100%;
            text-align: center;
            position: relative;
        }

        #bmiMeter {
            margin-top: 20px;
            position: relative;
        }

        #meterContainer {
            width: 80%;
            margin: 0 auto;
        }

        #meterRange {
            position: relative;
            height: 20px;
            background-color: #ccc; /* Gray background for the meter range */
            border-radius: 10px;
        }

        #meterArrow {
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 2px;
            height: 20px;
            background-color: #4caf50; /* Green arrow color */
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
        }

        form {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        label {
            margin-bottom: 5px;
        }

        input {
            margin-bottom: 10px;
            padding: 8px;
            width: 100%;
            box-sizing: border-box;
        }

        button {
            padding: 10px;
            background-color: #4caf50; /* Green button color */
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        h1, h2 {
            color: #333; /* Dark text color */
        }

        p {
            margin-top: 0;
            color: #666; /* Medium gray text color */
        }

        #causes {
            color: #FF0000; /* Red color for causes */
            font-weight: bold;
        }

        #effects {
            color: #0070ff; /* Blue color for effects */
            font-weight: bold;
        }

        #moreDetailsLink {
            color: #4285f4; /* Blue color for the link */
            text-decoration: underline;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div id="container">
        <h1>Health Tracker</h1>
        <form id="healthForm">
    <label for="height">Height (cm):</label>
    <input type="number" id="height" required min="50" max="250" onkeydown="if(event.key==='Enter') calculateBMI()">

    <label for="weight">Weight (kg):</label>
    <input type="number" id="weight" required min="10" max="500" onkeydown="if(event.key==='Enter') calculateBMI()">

    <label for="age">Age:</label>
    <input type="number" id="age" required min="1" max="120" onkeydown="if(event.key==='Enter') calculateBMI()">

    <button type="button" onclick="calculateBMI()">Calculate BMI</button>
</form>

        <h2>Results</h2>
        <p id="result"></p>
        <p id="effects"></p>
        <p id="plans"></p>
        <p id="causes"></p>
        <p id="dietPlanLink"></p>

        <div id="bmiMeter">
            <div id="meterContainer">
                <div id="meterRange"></div>
                <div id="meterArrow"></div>
            </div>
        </div>
    </div>

    <script>
        function calculateBMI() {
            // Get user inputs
            var height = document.getElementById('height').value;
            var weight = document.getElementById('weight').value;
            var age = document.getElementById('age').value;

            // Check if inputs are logical
            if (height < 50 || height > 250 || weight < 10 || weight > 500 || age < 1 || age > 120) {
                alert('Please enter valid height, weight, and age.');
                return;
            }

            // Calculate BMI
            var bmi = (weight / ((height / 100) * (height / 100))).toFixed(2);

            // Display BMI result
            document.getElementById('result').innerHTML = 'Your BMI: ' + bmi;

            // Display "Normal BMI" if BMI is within the normal range
            if (bmi >= 18.5 && bmi <= 24.9) {
                document.getElementById('effects').innerHTML = 'Normal BMI';
                document.getElementById('plans').innerHTML = 'Maintain a balanced diet and engage in regular physical activity to stay healthy.';
                // Clear any existing causes
                document.getElementById('causes').innerHTML = '';
                // Set the link for normal BMI
                document.getElementById('dietPlanLink').innerHTML = '<a href="https://www.verywellfit.com/an-example-of-a-healthy-balanced-meal-plan-2506647" target="_blank">Healthy Balanced Meal Plan</a>';
            } else {
                var effects = getEffects(bmi);
                document.getElementById('effects').innerHTML = effects ? 'Effects: ' + effects : '';

                var plans = getPlans(bmi);

                // Display causes
                var causes = getCauses(bmi);
                document.getElementById('causes').innerHTML = causes ? 'Causes: ' + causes : '';

                // Display the hyperlink for diet plan based on BMI category
                if (bmi < 18.5) {
                    plans += ' <a href="https://www.lybrate.com/topic/diet-for-underweight" target="_blank">Diet Plan</a>';
                } else if (bmi >= 25 && bmi <= 29.9) {
                    plans += ' <a href="https://www.medicalnewstoday.com/articles/weight-loss-meal-plan" target="_blank">Diet Plan</a>';
                } else if (bmi > 29.9) {
                    plans += ' <a href="https://www.sugarfit.com/blog/obesity-diet-plan/" target="_blank">Diet Plan</a>';
                }

                document.getElementById('plans').innerHTML = 'Plans: ' + plans;

                // Clear the diet plan link if it's not normal BMI
                document.getElementById('dietPlanLink').innerHTML = '';
            }

            // Update BMI meter
            updateBMIMeter(bmi);
        }

        function getEffects(bmi) {
            var effects = '';
            if (bmi < 18.5) {
                effects = 'You are underweight, which may lead to reduced energy levels and a weakened immune system. It is essential to consult with a healthcare professional.';
            } else if (bmi >= 25 && bmi <= 29.9) {
                effects = 'You are overweight, which may increase the risk of chronic diseases like heart disease and diabetes. Consider a balanced diet and regular exercise.';
            } else if (bmi > 29.9) {
                effects = 'You are obese, which may lead to various health problems. Consult with a healthcare professional for a personalized plan.';
            }
            return effects;
        }

        function getPlans(bmi) {
            var plans = '';
            if (bmi < 18.5) {
                plans = 'Consider a diet rich in proteins and healthy fats. Consult with a nutritionist for a personalized plan.';
            } else if (bmi >= 18.5 && bmi <= 24.9) {
                plans = 'Maintain a balanced diet and engage in regular physical activity to stay healthy.';
            } else if (bmi >= 25 && bmi <= 29.9) {
                plans = 'Focus on a calorie-controlled diet and increase your physical activity. Consult with a healthcare professional for guidance.';
            } else {
                plans = 'Consult with a healthcare professional for a comprehensive weight management plan tailored to your needs.';
            }
            return plans;
        }

        function getCauses(bmi) {
            var causes = '';
            if (bmi < 18.5) {
                causes = 'Possible causes of underweight include inadequate calorie intake, high metabolism, inadequate nutrition, underlying health conditions, or a combination of these factors. Sometimes, mental health issues like stress, anxiety, or depression can also contribute to weight loss or difficulty gaining weight.';
            } else if (bmi >= 25 && bmi <= 29.9) {
                causes = 'Possible causes of overweight include excess calorie intake, lack of physical activity, genetic factors, low-nutrient meals, sedentary lifestyles, and environmental effects such as easy availability of unhealthy foods.';
            } else if (bmi > 29.9) {
                causes = 'Possible causes of obesity include unhealthy eating habits, lack of physical activity, or genetic factors. Emotional eating and other psychological issues might also play a role.';
            }
            return causes;
        }

        function updateBMIMeter(bmi) {
            var arrowPosition = (bmi - 18.5) / (29.9 - 18.5) * 100;
            document.getElementById('meterArrow').style.left = arrowPosition + '%';
        }
    </script>
</body>
</html>
