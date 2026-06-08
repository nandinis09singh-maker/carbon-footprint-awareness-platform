# carbon-footprint-awareness-platform
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoTrack - Carbon Footprint Awareness Platform</title>
    <style>
        :root {
            --primary: #2e7d32;
            --primary-light: #e8f5e9;
            --dark: #1b5e20;
            --text: #333;
            --bg: #f9fbf9;
        }
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            margin: 0;
            padding: 0;
            background-color: var(--bg);
            color: var(--text);
            line-height: 1.6;
        }
        header {
            background-color: var(--primary);
            color: white;
            text-align: center;
            padding: 2rem 1rem;
        }
        main {
            max-width: 800px;
            margin: 2rem auto;
            padding: 0 1rem;
        }
        .card {
            background: white;
            border-radius: 12px;
            padding: 2rem;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
            margin-bottom: 2rem;
        }
        .form-group {
            margin-bottom: 1.5rem;
        }
        label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: bold;
        }
        input, select {
            width: 100%;
            padding: 0.75rem;
            border: 1px solid #ccc;
            border-radius: 6px;
            box-sizing: border-box;
            font-size: 1rem;
        }
        button {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 0.75rem 1.5rem;
            font-size: 1rem;
            border-radius: 6px;
            cursor: pointer;
            width: 100%;
            font-weight: bold;
            transition: background 0.2s;
        }
        button:hover {
            background-color: var(--dark);
        }
        .hidden {
            display: none;
        }
        .result-badge {
            text-align: center;
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--dark);
            background: var(--primary-light);
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 1.5rem;
        }
        ul {
            padding-left: 1.2rem;
        }
        li {
            margin-bottom: 0.75rem;
        }
    </style>
</head>
<body>

    <header>
        <h1>🌱 EcoTrack</h1>
        <p>Understand, track, and reduce your daily carbon footprint.</p>
    </header>

    <main>
        <!-- Tracker Form Component -->
        <div class="card" id="input-card">
            <h2>Calculate Your Footprint</h2>
            <form id="calc-form">
                <div class="form-group">
                    <label for="city">Your Location</label>
                    <input type="text" id="city" value="Mumbai" required>
                </div>
                <div class="form-group">
                    <label for="commute">Daily Commute Distance (km)</label>
                    <input type="number" id="commute" value="10" min="0" required>
                </div>
                <div class="form-group">
                    <label for="transport">Primary Mode of Transport</label>
                    <select id="transport">
                        <option value="bus">Public Bus</option>
                        <option value="train">Local Train / Metro</option>
                        <option value="auto">Auto / Cab</option>
                        <option value="bike">Two-Wheeler</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="ac">Daily Air Conditioning Usage (Hours)</label>
                    <input type="number" id="ac" value="4" min="0" max="24" required>
                </div>
                <button type="submit">Generate Personalized Plan</button>
            </form>
        </div>

        <!-- Insights & Actions Component -->
        <div class="card hidden" id="results-card">
            <h2>Your Personalized Insights</h2>
            <div class="result-badge" id="footprint-score">
                Estimated: 0 kg CO₂e / day
            </div>
            
            <h3>💡 Targeted Actions to Reduce Your Impact</h3>
            <ul id="action-list">
                <!-- Recommendations will be injected here dynamically -->
            </ul>
            
            <button onclick="resetForm()" style="background-color: #666; margin-top: 1rem;">Recalculate</button>
        </div>
    </main>

    <script>
        document.getElementById('calc-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // Get inputs
            const commute = parseFloat(document.getElementById('commute').value);
            const transport = document.getElementById('transport').value;
            const acHours = parseFloat(document.getElementById('ac').value);
            const city = document.getElementById('city').value;

            // Emission factors (approximate kg CO2e per km or hour)
            let transportFactor = 0.05; // default bus/train
            if (transport === 'auto') transportFactor = 0.12;
            if (transport === 'bike') transportFactor = 0.07;
            if (transport === 'train') transportFactor = 0.02; 

            const acFactor = 0.6; // average per hour for standard AC units

            // Calculate daily totals
            const travelEmissions = commute * transportFactor;
            const acEmissions = acHours * acFactor;
            const totalEmissions = (travelEmissions + acEmissions).toFixed(2);

            // Display results
            document.getElementById('footprint-score').innerText = `Estimated: ${totalEmissions} kg CO₂e / day`;
            
            // Generate Personalized Insights
            const actionList = document.getElementById('action-list');
            actionList.innerHTML = ''; // clear old list

            // Action 1: Dynamic AC advice
            if (acHours > 0) {
                actionList.innerHTML += `<li><strong>Optimize Your AC:</strong> Turn your AC up to 24°C and use a ceiling fan. This reduces cooling energy consumption by up to 30% without losing comfort.</li>`;
            }
            
            // Action 2: Dynamic Transport advice
            if (transport === 'bus' || transport === 'auto') {
                actionList.innerHTML += `<li><strong>Eco-Friendly Transit:</strong> Since you travel ${commute} km, try shifting legs of your trip to the local electric rail or Metro to minimize transit gridlock emissions.</li>`;
            } else {
                actionList.innerHTML += `<li><strong>Active Commuting:</strong> Keep utilizing shared electric transits. Consider carpooling or walking for short-trip errands under 1 km.</li>`;
            }

            // Action 3, 4, 5: General digital & lifestyle hacks
            actionList.innerHTML += `<li><strong>Vampire Power Drop:</strong> Unplug laptop and phone chargers before heading off to classes. Idle plugs draw silent "standby" power all day.</li>`;
            actionList.innerHTML += `<li><strong>Low-Carbon Lunches:</strong> Swap out meat or heavy dairy for plant-based alternatives (like Dal Chawal or local sprouts) just 2 days a week to slash your food production footprint.</li>`;
            actionList.innerHTML += `<li><strong>Digital Housekeeping:</strong> Clean out stale repository branches and unneeded cloud forks on GitHub to save server storage energy.</li>`;

            // Swap view states
            document.getElementById('input-card').classList.add('hidden');
            document.getElementById('results-card').classList.remove('hidden');
        });

        function resetForm() {
            document.getElementById('input-card').classList.remove('hidden');
            document.getElementById('results-card').classList.add('hidden');
        }
    </script>
</body>
</html>
