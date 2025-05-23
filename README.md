<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nexus BMI Calculator</title>
    <style>
        :root {
            --primary: #6a5acd;       /* Slate blue */
            --secondary: #483d8b;     /* Dark slate blue */
            --accent: #9370db;        /* Medium purple */
            --dark: #121212;          /* Dark background */
            --darker: #0a0a0a;        /* Even darker */
            --light: #e0e0e0;         /* Light text */
            --lighter: #f5f5f5;       /* Very light text */
            --success: #4caf50;       /* Green */
            --warning: #ffa500;       /* Orange */
            --danger: #ff4500;        /* Orange-red */
            --info: #4169e1;          /* Royal blue */
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', sans-serif;
        }
        
        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Changed from center to flex-start */
            padding: 20px;
            position: relative;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
            color: var(--light);
        }
        
        #particles-js {
            position: fixed; /* Changed from absolute to fixed */
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            z-index: 0;
        }
        
        .container {
            background: rgba(18, 18, 18, 0.85);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3), 
                        0 0 0 1px rgba(255, 255, 255, 0.05);
            width: 100%;
            max-width: 800px;
            overflow: hidden;
            animation: fadeIn 0.8s cubic-bezier(0.22, 1, 0.36, 1);
            position: relative;
            z-index: 1;
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            margin: 20px 0; /* Added margin for scroll space */
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, rgba(255,255,255,0) 70%);
            transform: rotate(30deg);
            animation: shine 8s infinite linear;
        }
        
        @keyframes shine {
            0% { transform: rotate(30deg) translate(-30%, -30%); }
            100% { transform: rotate(30deg) translate(30%, 30%); }
        }
        
        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            position: relative;
            text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        }
        
        .header p {
            font-size: 1.05rem;
            opacity: 0.9;
            position: relative;
        }
        
        .content {
            padding: 30px;
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .input-section {
            display: flex;
            flex-wrap: wrap;
            gap: 25px;
            justify-content: space-between;
        }
        
        .input-group {
            flex: 1;
            min-width: 250px;
        }
        
        .input-group label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: var(--lighter);
            font-size: 0.95rem;
        }
        
        .input-group input, .input-group select {
            width: 100%;
            padding: 14px 18px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s ease-out;
            background-color: rgba(30, 30, 30, 0.7);
            color: var(--lighter);
            box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.3);
        }
        
        .input-group input:focus, .input-group select:focus {
            border-color: var(--accent);
            outline: none;
            box-shadow: 0 0 0 3px rgba(147, 112, 219, 0.3),
                        inset 0 1px 3px rgba(0, 0, 0, 0.3);
            background-color: rgba(40, 40, 40, 0.7);
        }
        
        .input-group input::placeholder {
            color: rgba(255, 255, 255, 0.3);
        }
        
        .unit-toggle {
            display: flex;
            gap: 12px;
            margin-top: 12px;
        }
        
        .unit-toggle button {
            flex: 1;
            padding: 10px;
            background-color: rgba(40, 40, 40, 0.7);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s ease-out;
            color: var(--light);
            font-size: 0.9rem;
        }
        
        .unit-toggle button:hover {
            background-color: rgba(60, 60, 60, 0.7);
        }
        
        .unit-toggle button.active {
            background-color: var(--primary);
            color: white;
            border-color: var(--primary);
            box-shadow: 0 2px 10px rgba(106, 90, 205, 0.3);
        }
        
        .calculate-btn {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            border: none;
            padding: 16px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.22, 1, 0.36, 1);
            margin-top: 15px;
            width: 100%;
            position: relative;
            overflow: hidden;
            z-index: 1;
            box-shadow: 0 4px 15px rgba(106, 90, 205, 0.3);
        }
        
        .calculate-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--secondary), var(--primary));
            opacity: 0;
            z-index: -1;
            transition: opacity 0.3s ease-out;
        }
        
        .calculate-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(106, 90, 205, 0.4);
        }
        
        .calculate-btn:hover::before {
            opacity: 1;
        }
        
        .calculate-btn:active {
            transform: translateY(1px);
        }
        
        .result-section {
            background: rgba(30, 30, 30, 0.6);
            border-radius: 12px;
            padding: 25px;
            text-align: center;
            display: none;
            animation: slideUp 0.6s cubic-bezier(0.22, 1, 0.36, 1);
            border: 1px solid rgba(255, 255, 255, 0.05);
            box-shadow: inset 0 1px 10px rgba(0, 0, 0, 0.2);
        }
        
        @keyframes slideUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .result-section h2 {
            margin-bottom: 15px;
            color: var(--lighter);
            font-size: 1.5rem;
        }
        
        .bmi-value {
            font-size: 3.8rem;
            font-weight: 700;
            margin: 15px 0;
            background: linear-gradient(135deg, var(--accent), var(--primary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 2px 10px rgba(106, 90, 205, 0.2);
        }
        
        .bmi-category {
            font-size: 1.6rem;
            font-weight: 600;
            margin-bottom: 20px;
            padding: 8px 20px;
            border-radius: 50px;
            display: inline-block;
        }
        
        .bmi-advice {
            font-size: 1.05rem;
            line-height: 1.7;
            margin-bottom: 20px;
            color: rgba(255, 255, 255, 0.8);
        }
        
        .bmi-scale {
            width: 100%;
            height: 35px;
            background: linear-gradient(to right, var(--danger), var(--warning), var(--success), var(--warning), var(--danger));
            border-radius: 20px;
            margin: 25px 0;
            position: relative;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }
        
        .bmi-scale::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(to bottom, rgba(255,255,255,0.1), rgba(255,255,255,0));
            border-radius: 20px;
        }
        
        .bmi-marker {
            position: absolute;
            top: -15px;
            width: 6px;
            height: 60px;
            background-color: white;
            transform: translateX(-50%);
            z-index: 2;
            border-radius: 3px;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.7);
            transition: left 0.8s cubic-bezier(0.22, 1, 0.36, 1);
        }
        
        .health-tips {
            text-align: left;
            margin-top: 25px;
            padding: 20px;
            background: rgba(40, 40, 40, 0.6);
            border-radius: 12px;
            box-shadow: 0 3px 15px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }
        
        .health-tips h3 {
            color: var(--accent);
            margin-bottom: 15px;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .health-tips h3::before {
            content: '💡';
        }
        
        .health-tips ul {
            padding-left: 25px;
        }
        
        .health-tips li {
            margin-bottom: 12px;
            color: rgba(255, 255, 255, 0.8);
            position: relative;
            line-height: 1.6;
        }
        
        .health-tips li::marker {
            color: var(--accent);
        }
        
        .category-underweight { 
            background-color: rgba(65, 105, 225, 0.15); 
            color: var(--info);
            border: 1px solid rgba(65, 105, 225, 0.3);
        }
        .category-normal { 
            background-color: rgba(76, 175, 80, 0.15);
            color: var(--success);
            border: 1px solid rgba(76, 175, 80, 0.3);
        }
        .category-overweight { 
            background-color: rgba(255, 165, 0, 0.15);
            color: var(--warning);
            border: 1px solid rgba(255, 165, 0, 0.3);
        }
        .category-obese { 
            background-color: rgba(255, 69, 0, 0.15);
            color: var(--danger);
            border: 1px solid rgba(255, 69, 0, 0.3);
        }
        
        footer {
            text-align: center;
            padding: 20px;
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.85rem;
            border-top: 1px solid rgba(255, 255, 255, 0.08);
        }
        
        /* Floating animation for the container */
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        .container:hover {
            animation: float 4s ease-in-out infinite;
        }
        
        /* Responsive adjustments */
        @media (max-width: 768px) {
            .header h1 {
                font-size: 2rem;
            }
            
            .content {
                padding: 25px;
            }
            
            .bmi-value {
                font-size: 3rem;
            }
            
            .bmi-category {
                font-size: 1.3rem;
            }
        }
        
        @media (max-width: 480px) {
            body {
                padding: 15px;
            }
            
            .header {
                padding: 25px 20px;
            }
            
            .header h1 {
                font-size: 1.8rem;
            }
            
            .content {
                padding: 20px;
            }
            
            .input-section {
                gap: 20px;
            }
            
            .input-group {
                min-width: 100%;
            }
            
            .bmi-value {
                font-size: 2.5rem;
            }
        }

        /* Loading animation */
        .loader {
            display: inline-block;
            width: 16px;
            height: 16px;
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
            margin-right: 8px;
            vertical-align: middle;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Error message styling */
        .error-message {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #ff4444;
            color: white;
            padding: 15px 25px;
            border-radius: 8px;
            box-shadow: 0 4px 15px rgba(255, 0, 0, 0.3);
            z-index: 1000;
            animation: fadeInOut 3s ease-in-out;
        }

        @keyframes fadeInOut {
            0%, 100% { opacity: 0; transform: translateX(-50%) translateY(-20px); }
            10%, 90% { opacity: 1; transform: translateX(-50%) translateY(0); }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
</head>
<body>
    <div id="particles-js"></div>
    
    <div class="container">
        <div class="header">
            <h1>Nexus BMI Calculator</h1>
            <p>Discover your Body Mass Index with our advanced health analyzer</p>
        </div>
        
        <div class="content">
            <div class="input-section">
                <div class="input-group">
                    <label for="weight">Weight</label>
                    <input type="number" id="weight" placeholder="Enter your weight" step="0.1" min="0">
                    <div class="unit-toggle">
                        <button id="kg-btn" class="active">Kilograms (kg)</button>
                        <button id="lbs-btn">Pounds (lbs)</button>
                    </div>
                </div>
                
                <div class="input-group">
                    <label for="height">Height</label>
                    <div id="height-metric">
                        <input type="number" id="height-cm" placeholder="Height in centimeters" min="0">
                    </div>
                    <div id="height-imperial" style="display: none;">
                        <div style="display: flex; gap: 12px;">
                            <input type="number" id="height-ft" placeholder="Feet" min="0" style="flex: 1;">
                            <input type="number" id="height-in" placeholder="Inches" min="0" max="11" style="flex: 1;">
                        </div>
                    </div>
                    <div class="unit-toggle">
                        <button id="metric-btn" class="active">Metric (cm)</button>
                        <button id="imperial-btn">Imperial (ft/in)</button>
                    </div>
                </div>
            </div>
            
            <button id="calculate-btn" class="calculate-btn">
                <span>Calculate BMI</span>
            </button>
            
            <div id="result-section" class="result-section">
                <h2>Your Health Analysis</h2>
                <div id="bmi-value" class="bmi-value">--</div>
                <div id="bmi-category" class="bmi-category">--</div>
                
                <div class="bmi-scale">
                    <div id="bmi-marker" class="bmi-marker" style="left: 50%;"></div>
                </div>
                
                <div id="bmi-advice" class="bmi-advice"></div>
                
                <div class="health-tips">
                    <h3>Personalized Health Recommendations</h3>
                    <ul id="health-tips-list">
                        <!-- Tips will be added dynamically -->
                    </ul>
                </div>
            </div>
        </div>
        
        <footer>
            <p>Note: BMI is a screening measure that may not account for muscle mass, bone density, or overall body composition. Consult a healthcare professional for comprehensive health assessment.</p>
            <br>
            <p> © Powered by D1-CSE-SIST</p>
        </footer>
    </div>

    <!-- Load particles.js library -->
    <script src="https://cdn.jsdelivr.net/particles.js/2.0.0/particles.min.js"></script>
    
    <script>
        // Initialize particles.js with more interactive settings
        document.addEventListener('DOMContentLoaded', function() {
            particlesJS('particles-js', {
                "particles": {
                    "number": {
                        "value": 100,
                        "density": {
                            "enable": true,
                            "value_area": 800
                        }
                    },
                    "color": {
                        "value": "#6a5acd"
                    },
                    "shape": {
                        "type": "circle",
                        "stroke": {
                            "width": 0,
                            "color": "#000000"
                        },
                        "polygon": {
                            "nb_sides": 5
                        }
                    },
                    "opacity": {
                        "value": 0.7,
                        "random": true,
                        "anim": {
                            "enable": true,
                            "speed": 1,
                            "opacity_min": 0.2,
                            "sync": false
                        }
                    },
                    "size": {
                        "value": 4,
                        "random": true,
                        "anim": {
                            "enable": true,
                            "speed": 3,
                            "size_min": 1,
                            "sync": false
                        }
                    },
                    "line_linked": {
                        "enable": true,
                        "distance": 130,
                        "color": "#6a5acd",
                        "opacity": 0.4,
                        "width": 1.5
                    },
                    "move": {
                        "enable": true,
                        "speed": 2.5,
                        "direction": "none",
                        "random": true,
                        "straight": false,
                        "out_mode": "out",
                        "bounce": false,
                        "attract": {
                            "enable": true,
                            "rotateX": 800,
                            "rotateY": 1600
                        }
                    }
                },
                "interactivity": {
                    "detect_on": "canvas",
                    "events": {
                        "onhover": {
                            "enable": true,
                            "mode": "repulse"
                        },
                        "onclick": {
                            "enable": true,
                            "mode": "bubble"
                        },
                        "resize": true
                    },
                    "modes": {
                        "grab": {
                            "distance": 150,
                            "line_linked": {
                                "opacity": 1
                            }
                        },
                        "bubble": {
                            "distance": 200,
                            "size": 15,
                            "duration": 1,
                            "opacity": 0.8,
                            "speed": 3
                        },
                        "repulse": {
                            "distance": 100,
                            "duration": 0.5
                        },
                        "push": {
                            "particles_nb": 6
                        },
                        "remove": {
                            "particles_nb": 3
                        }
                    }
                },
                "retina_detect": true
            });
        });

        // DOM Elements
        const kgBtn = document.getElementById('kg-btn');
        const lbsBtn = document.getElementById('lbs-btn');
        const metricBtn = document.getElementById('metric-btn');
        const imperialBtn = document.getElementById('imperial-btn');
        const heightMetric = document.getElementById('height-metric');
        const heightImperial = document.getElementById('height-imperial');
        const calculateBtn = document.getElementById('calculate-btn');
        const resultSection = document.getElementById('result-section');
        const bmiValue = document.getElementById('bmi-value');
        const bmiCategory = document.getElementById('bmi-category');
        const bmiMarker = document.getElementById('bmi-marker');
        const bmiAdvice = document.getElementById('bmi-advice');
        const healthTipsList = document.getElementById('health-tips-list');
        
        // Unit toggle functionality with animation
        kgBtn.addEventListener('click', () => {
            if (!kgBtn.classList.contains('active')) {
                kgBtn.classList.add('active');
                lbsBtn.classList.remove('active');
                animateButton(kgBtn);
            }
        });
        
        lbsBtn.addEventListener('click', () => {
            if (!lbsBtn.classList.contains('active')) {
                lbsBtn.classList.add('active');
                kgBtn.classList.remove('active');
                animateButton(lbsBtn);
            }
        });
        
        metricBtn.addEventListener('click', () => {
            if (!metricBtn.classList.contains('active')) {
                metricBtn.classList.add('active');
                imperialBtn.classList.remove('active');
                heightMetric.style.display = 'block';
                heightImperial.style.display = 'none';
                animateButton(metricBtn);
            }
        });
        
        imperialBtn.addEventListener('click', () => {
            if (!imperialBtn.classList.contains('active')) {
                imperialBtn.classList.add('active');
                metricBtn.classList.remove('active');
                heightMetric.style.display = 'none';
                heightImperial.style.display = 'block';
                animateButton(imperialBtn);
            }
        });
        
        function animateButton(button) {
            button.style.transform = 'scale(0.95)';
            setTimeout(() => {
                button.style.transform = 'scale(1)';
            }, 150);
        }
        
        // Calculate BMI with loading animation
        calculateBtn.addEventListener('click', function() {
            // Add loading animation
            this.innerHTML = '<span class="loader"></span>Calculating...';
            this.disabled = true;
            
            setTimeout(() => {
                calculateBMI();
                this.innerHTML = '<span>Calculate BMI</span>';
                this.disabled = false;
            }, 800);
        });
        
        function calculateBMI() {
            // Get weight
            let weight = parseFloat(document.getElementById('weight').value);
            
            // Convert weight to kg if in lbs
            if (lbsBtn.classList.contains('active')) {
                weight = weight * 0.453592;
            }
            
            // Get height
            let height;
            if (metricBtn.classList.contains('active')) {
                height = parseFloat(document.getElementById('height-cm').value) / 100; // convert cm to m
            } else {
                const feet = parseFloat(document.getElementById('height-ft').value) || 0;
                const inches = parseFloat(document.getElementById('height-in').value) || 0;
                height = (feet * 12 + inches) * 0.0254; // convert ft+in to m
            }
            
            // Validate inputs
            if (isNaN(weight) || isNaN(height) || weight <= 0 || height <= 0) {
                showError('Please enter valid weight and height values.');
                return;
            }
            
            // Calculate BMI
            const bmi = weight / (height * height);
            
            // Display result
            displayResult(bmi);
        }
        
        function showError(message) {
            const errorDiv = document.createElement('div');
            errorDiv.className = 'error-message';
            errorDiv.textContent = message;
            errorDiv.style.cssText = `
                position: fixed;
                top: 20px;
                left: 50%;
                transform: translateX(-50%);
                background-color: #ff4444;
                color: white;
                padding: 15px 25px;
                border-radius: 8px;
                box-shadow: 0 4px 15px rgba(255, 0, 0, 0.3);
                z-index: 1000;
                animation: fadeInOut 3s ease-in-out;
            `;
            
            document.body.appendChild(errorDiv);
            
            setTimeout(() => {
                errorDiv.remove();
            }, 3000);
        }
        
        function displayResult(bmi) {
            resultSection.style.display = 'block';
            bmiValue.textContent = bmi.toFixed(1);
            
            // Determine category and advice
            let category, advice, categoryClass, particleColor;
            
            if (bmi < 18.5) {
                category = 'Underweight';
                advice = 'Your analysis suggests you may be underweight. This could indicate nutritional deficiency or other health considerations. We recommend consulting with a healthcare professional to develop a personalized wellness plan.';
                categoryClass = 'category-underweight';
                particleColor = '#4169e1';
                
                // Health tips for underweight
                healthTipsList.innerHTML = `
                    <li><strong>Nutrition:</strong> Focus on nutrient-dense foods like nuts, seeds, avocados, whole grains, and lean proteins</li>
                    <li><strong>Meal Frequency:</strong> Consider 5-6 smaller meals throughout the day if you have a small appetite</li>
                    <li><strong>Exercise:</strong> Incorporate strength training to build healthy muscle mass</li>
                    <li><strong>Monitoring:</strong> Keep a food diary to ensure adequate calorie and nutrient intake</li>
                    <li><strong>Professional Support:</strong> Consult a dietitian for personalized meal planning</li>
                `;
            } 
            else if (bmi >= 18.5 && bmi < 25) {
                category = 'Healthy Range';
                advice = 'Excellent! Your analysis falls within the healthy range. Maintaining this balance supports overall wellness and reduces risk for many health conditions. Continue your healthy habits and regular health check-ups.';
                categoryClass = 'category-normal';
                particleColor = '#4caf50';
                
                // Health tips for normal weight
                healthTipsList.innerHTML = `
                    <li><strong>Maintenance:</strong> Continue balanced nutrition with variety of fruits, vegetables, and whole foods</li>
                    <li><strong>Activity:</strong> Maintain at least 150 minutes of moderate exercise weekly</li>
                    <li><strong>Hydration:</strong> Drink adequate water and limit sugary beverages</li>
                    <li><strong>Prevention:</strong> Schedule regular health screenings and check-ups</li>
                    <li><strong>Lifestyle:</strong> Practice stress management and quality sleep habits</li>
                `;
            } 
            else if (bmi >= 25 && bmi < 30) {
                category = 'Overweight';
                advice = 'Your analysis indicates you may be overweight. This can increase health risks, but positive changes can make a significant difference. Focus on sustainable lifestyle improvements in nutrition and activity.';
                categoryClass = 'category-overweight';
                particleColor = '#ffa500';
                
                // Health tips for overweight
                healthTipsList.innerHTML = `
                    <li><strong>Portion Control:</strong> Practice mindful eating and appropriate portion sizes</li>
                    <li><strong>Food Choices:</strong> Increase vegetables, fruits, and fiber while reducing processed foods</li>
                    <li><strong>Movement:</strong> Gradually increase daily activity - aim for 30 minutes most days</li>
                    <li><strong>Behavior:</strong> Identify and address emotional eating patterns if present</li>
                    <li><strong>Support:</strong> Consider joining a wellness program for guidance</li>
                `;
            } 
            else {
                category = 'Obese';
                advice = 'Your analysis suggests obesity, which significantly impacts health. The good news is that even modest weight loss (5-10%) can provide important health benefits. We strongly recommend consulting with healthcare professionals to develop a comprehensive, sustainable plan.';
                categoryClass = 'category-obese';
                particleColor = '#ff4500';
                
                // Health tips for obese
                healthTipsList.innerHTML = `
                    <li><strong>Goal Setting:</strong> Start with small, achievable targets for sustainable progress</li>
                    <li><strong>Professional Guidance:</strong> Work with a healthcare team for personalized support</li>
                    <li><strong>Nutrition:</strong> Focus on whole, unprocessed foods and balanced meals</li>
                    <li><strong>Activity:</strong> Find enjoyable ways to increase daily movement</li>
                    <li><strong>Holistic Approach:</strong> Address sleep, stress, and emotional factors</li>
                `;
            }
            
            bmiCategory.textContent = category;
            bmiCategory.className = 'bmi-category ' + categoryClass;
            bmiAdvice.textContent = advice;
            
            // Position the marker on the scale with smooth animation
            let markerPosition;
            if (bmi < 15) markerPosition = 0;
            else if (bmi > 40) markerPosition = 100;
            else markerPosition = ((bmi - 15) / (40 - 15)) * 100;
            
            bmiMarker.style.left = `${markerPosition}%`;
            
            // Scroll to results with smooth behavior
            resultSection.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
            
            // Update particles color based on BMI category
            if (window.pJSDom && window.pJSDom.length > 0) {
                const pJS = window.pJSDom[0].pJS;
                pJS.particles.color.value = particleColor;
                pJS.particles.line_linked.color = particleColor;
                pJS.fn.particlesRefresh();
            }
            
            // Add confetti celebration for healthy BMI
            if (categoryClass === 'category-normal') {
                triggerConfetti();
            }
        }
        
        function triggerConfetti() {
            const colors = ['#6a5acd', '#9370db', '#4caf50', '#4169e1', '#ffa500'];
            
            for (let i = 0; i < 100; i++) {
                const confetti = document.createElement('div');
                confetti.style.position = 'fixed';
                confetti.style.width = '8px';
                confetti.style.height = '8px';
                confetti.style.backgroundColor = colors[Math.floor(Math.random() * colors.length)];
                confetti.style.borderRadius = '50%';
                confetti.style.left = Math.random() * 100 + 'vw';
                confetti.style.top = '-10px';
                confetti.style.zIndex = '1000';
                confetti.style.opacity = '0.8';
                confetti.style.transform = 'rotate(' + Math.random() * 360 + 'deg)';
                
                document.body.appendChild(confetti);
                
                const animation = confetti.animate([
                    { top: '-10px', opacity: 0.8 },
                    { top: '100vh', opacity: 0 }
                ], {
                    duration: Math.random() * 3000 + 2000,
                    easing: 'cubic-bezier(0.1, 0.8, 0.9, 1)'
                });
                
                animation.onfinish = () => confetti.remove();
            }
        }
        
        // Add input validation
        document.querySelectorAll('input[type="number"]').forEach(input => {
            input.addEventListener('input', (e) => {
                if (e.target.value < 0) e.target.value = '';
            });
        });
    </script>
</body>
</html>
