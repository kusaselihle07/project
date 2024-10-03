<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>World Clock</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }

        .container {
            text-align: center;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            margin-bottom: 20px;
        }

        select {
            padding: 10px;
            margin: 20px 0;
        }

        footer {
            margin-top: 20px;
            font-size: 0.9em;
            color: #666;
        }

        a {
            text-decoration: none;
            color: #8064e9;
        }

        a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>World Clock</h1>
        <div class="location-selection">
            <label for="location">Select a Location:</label>
            <select id="location" onchange="updateTime()">
                <option value="America/New_York">New York</option>
                <option value="Europe/London">London</option>
                <option value="Asia/Tokyo">Tokyo</option>
                <option value="Australia/Sydney">Sydney</option>
            </select>
        </div>
        <div class="time-display">
            <h2>Current Time:</h2>
            <div id="current-time"></div>
        </div>
        <div class="current-location-time">
            <h2>Your Local Time:</h2>
            <div id="local-time"></div>
        </div>
        <footer>
            This project was coded with ❤️ by Kusaselihle Nene and is open sourced on <a href="https://github.com/kusaselihle07" target="_blank">GitHub</a>.
        </footer>
    </div>
    <script>
        function updateTime() {
            const locationSelect = document.getElementById("location");
            const selectedLocation = locationSelect.value;
            const currentTimeElement = document.getElementById("current-time");

            const options = {
                timeZone: selectedLocation,
                hour: "2-digit",
                minute: "2-digit",
                second: "2-digit",
                hour12: false,
            };
            
            const currentTime = new Intl.DateTimeFormat("en-US", options).format(new Date());
            currentTimeElement.innerHTML = currentTime;

            // Update local time
            updateLocalTime();
        }

        function updateLocalTime() {
            const localTimeElement = document.getElementById("local-time");
            const localTime = new Date().toLocaleTimeString();
            localTimeElement.innerHTML = localTime;
        }

        // Initialize the clock
        updateTime();
        setInterval(updateTime, 1000); // Update the time every second
    </script>
</body>
</html>