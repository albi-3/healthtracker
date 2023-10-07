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
            <input type="number" id="height" required>

            <label for="weight">Weight (kg):</label>
            <input type="number" id="weight" required>

            <label for="age">Age:</label>
            <input type="number" id="age" required>

            <button type="button" onclick="calculateBMI()">Calculate BMI</button>
        </form>

        <h2>Results</h2>
        <p id="result"></p>
        <p id="effects"></p>
        <p id="plans"></p>
        <p id="causes"></p>

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

            // Check if all inputs are provided
            if (!height || !weight || !age) {
                alert('Please enter all the required information.');
                return;
            }

            // Calculate BMI
            var bmi = (weight / ((height / 100) * (height / 100))).toFixed(2);

            // Display BMI result
            document.getElementById('result').innerHTML = 'Your BMI: ' + bmi;

            // Display effects based on BMI
            var effects = getEffects(bmi);
            document.getElementById('effects').innerHTML = 'Effects: ' + effects;

            // Display diet and workout plans based on BMI category
            var plans = getPlans(bmi);
            document.getElementById('plans').innerHTML = 'Plans: ' + plans;

            // Display causes based on BMI
            var causes = getCauses(bmi);
            document.getElementById('causes').innerHTML = causes ? 'Causes: ' + causes : '';

            // Update BMI meter
            updateBMIMeter(bmi);
        }

        function getEffects(bmi) {
            // Add your logic to determine effects based on BMI
            // Example logic: Just for illustration, not accurate
            if (bmi < 18.5) {
                return 'Underweight. Your normal BMI is 18.5.';
            } else if (bmi >= 18.5 && bmi <= 24.9) {
                return 'Normal weight';
            } else if (bmi >= 25 && bmi <= 29.9) {
                return 'Overweight. Your normal BMI is 24.9.';
            } else {
                return 'Obese. Your normal BMI is 24.9.';
            }
        }

        function getPlans(bmi) {
            // Add your logic to determine plans based on BMI
            // Example logic: Just for illustration, not accurate
            if (bmi < 18.5) {
                return 'Follow a diet rich in protein and healthy fats. Consider weight training.';
            } else if (bmi >= 18.5 && bmi <= 24.9) {
                return 'Maintain a balanced diet and regular exercise routine.';
            } else if (bmi >= 25 && bmi <= 29.9) {
                return 'Focus on a calorie-controlled diet and increase physical activity.';
            } else {
                return 'Consult with a healthcare professional for personalized advice.';
            }
        }

        function getCauses(bmi) {
            // Add your logic to determine causes based on BMI
            // Example logic: Just for illustration, not accurate
            if (bmi < 18.5) {
                return 'Possible causes of underweight include inadequate calorie intake, high metabolism, inadequate nutrition, underlying health conditions, or a combination of these factors. Sometimes, mental health issues like stress, anxiety, or depression can also contribute to weight loss or difficulty gaining weight. It\'s essential to address the root cause and work with healthcare professionals, such as a doctor or nutritionist, to develop a healthy plan for weight management. <a id="moreDetailsLink" href="https://www.medicalnewstoday.com/articles/321612" target="_blank">More details</a>';
            } else if (bmi >= 25 && bmi <= 29.9) {
                return 'Possible causes of overweight include excess calorie intake, lack of physical activity,genetic factors low-nutrient meals, sedentary lifestyles and environmental effects such as easy availability to unhealthy foods all contribute to this imbalance. <a id="moreDetailsLink" href="https://www.cdc.gov/obesity/basics/causes.html" target="_blank">More details</a>';
            } else if (bmi > 29.9) {
                return 'Possible causes of obesity include unhealthy eating habits, lack of physical activity, or genetic factors.Emotional eating and other psychological issues might also play a role. <a id="moreDetailsLink" href="https://www.mayoclinic.org/diseases-conditions/obesity/symptoms-causes/syc-20375742" target="_blank">More details</a>';
            } else {
                return ''; // Return empty string for normal weight
            }
        }

        function updateBMIMeter(bmi) {
            // Calculate the position of the arrow based on the BMI value
            var arrowPosition = (bmi - 18.5) / (29.9 - 18.5) * 100;

            // Update the arrow position
            document.getElementById('meterArrow').style.left = arrowPosition + '%';
        }
    </script>
</body>
</html>
