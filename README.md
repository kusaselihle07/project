<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="true">
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            background-color: #f4f4f9;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
            position: relative;
        }

        .weather-app {
            background-color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            width: 100%;
            text-align: center;
            position: relative;
            z-index: 1;
        }

        .search-form {
            margin-bottom: 20px;
        }

        .search-form-input {
            padding: 10px;
            width: 75%;
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        .search-form-button {
            padding: 10px 20px;
            border: none;
            background-color: #8064e9;
            color: white;
            border-radius: 5px;
            cursor: pointer;
        }

        .weather-app-city {
            font-size: 32px;
            margin: 10px 0;
        }

        .weather-app-details {
            font-size: 18px;
            color: #555;
        }

        .weather-app-temperature-container {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 20px;
        }

        .weather-app-temperature {
            font-size: 48px;
            margin-right: 10px;
        }

        .weather-app-unit {
            font-size: 24px;
        }

        footer {
            margin-top: 20px;
            font-size: 14px;
        }

        footer a {
            color: #8064e9;
            text-decoration: none;
        }

        footer a:hover {
            text-decoration: underline;
        }

        /* Cloud icon based on temperature */
        .cloud {
            position: absolute;
            background: white;
            border-radius: 50%;
            opacity: 0.8;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.8);
        }

        .cloud-1, .cloud-2, .cloud-3, .cloud-4, .cloud-5 {
            width: 80px;
            height: 50px;
            animation: moveCloud 25s linear infinite;
        }

        .cloud-1 { top: 20%; left: 5%; }
        .cloud-2 { top: 25%; left: 20%; }
        .cloud-3 { top: 30%; left: 35%; }
        .cloud-4 { top: 35%; left: 50%; }
        .cloud-5 { top: 40%; left: 65%; }

        @keyframes moveCloud {
            0% { transform: translateX(0); }
            100% { transform: translateX(100vw); }
        }

        /* Hide all clouds by default */
        .cloud { display: none; }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/axios@1.1.2/dist/axios.min.js"></script>
    <title>Meteo App</title>
</head>

<body>
    <div class="cloud cloud-1"></div>
    <div class="cloud cloud-2"></div>
    <div class="cloud cloud-3"></div>
    <div class="cloud cloud-4"></div>
    <div class="cloud cloud-5"></div>

    <div class="weather-app">
        <header>
            <form class="search-form" id="search-form">
                <input type="search" placeholder="Enter a city..." required id="search-form-input" class="search-form-input">
                <input type="submit" value="Search" class="search-form-button">
            </form>
        </header>

        <main>
            <div class="weather-app-data">
                <div>
                    <h1 class="weather-app-city" id="city">Weather</h1>
                    <p class="weather-app-details">
                        <span id="time">Date & Time</span>,
                        <span id="description">Description</span>
                        <br>
                        Humidity: <strong id="humidity">--</strong>%, Wind: <strong id="wind-speed">--</strong> km/h
                    </p>
                </div>
                <div class="weather-app-temperature-container">
                    <img id="icon" alt="Weather icon" width="50" height="50">
                    <div class="weather-app-temperature" id="temperature">--</div>
                    <div class="weather-app-unit">°C</div>
                </div>
            </div>
        </main>

        <footer>
            This project was coded by
            <a href="https://github.com/Kusaselihle07" target="_blank">Kusaselihle Nene</a> and is
            <a href="https://github.com/KusaselihleNene/meteo-app" target="_blank">open-sourced on GitHub</a> and
            <a href="https://meteo-kusaselihle.netlify.app/" target="_blank">hosted on Netlify</a>.
        </footer>
    </div>

    <script>
        const apiKey = '269b1b60accc8b461b6a352e60ca6853';  // Your OpenWeatherMap API Key
        const form = document.getElementById('search-form');
        const input = document.getElementById('search-form-input');
        const cityElement = document.getElementById('city');
        const descriptionElement = document.getElementById('description');
        const humidityElement = document.getElementById('humidity');
        const windSpeedElement = document.getElementById('wind-speed');
        const temperatureElement = document.getElementById('temperature');
        const iconElement = document.getElementById('icon');
        const timeElement = document.getElementById('time');
        const clouds = document.querySelectorAll('.cloud');

        form.addEventListener('submit', function (e) {
            e.preventDefault();
            const city = input.value.trim();
            if (city) {
                fetchWeatherData(city);
            }
        });

        function fetchWeatherData(city) {
            const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=metric&appid=${apiKey}`;

            axios.get(url)
                .then(response => {
                    const weatherData = response.data;
                    cityElement.textContent = weatherData.name;
                    descriptionElement.textContent = weatherData.weather[0].description;
                    humidityElement.textContent = weatherData.main.humidity;
                    windSpeedElement.textContent = weatherData.wind.speed;
                    temperatureElement.textContent = Math.round(weatherData.main.temp);
                    const iconCode = weatherData.weather[0].icon;
                    iconElement.src = `http://openweathermap.org/img/wn/${iconCode}@2x.png`;

                    // Update date and time
                    const date = new Date();
                    const options = { year: 'numeric', month: 'long', day: 'numeric', hour: 'numeric', minute: 'numeric', second: 'numeric', timeZoneName: 'short' };
                    timeElement.textContent = date.toLocaleString('en-US', options);

                    // Update clouds based on temperature
                    updateClouds(weatherData.main.temp);
                })
                .catch(error => {
                    console.error('Error fetching weather data:', error);
                    cityElement.textContent = 'City not found';
                    descriptionElement.textContent = '';
                    humidityElement.textContent = '--';
                    windSpeedElement.textContent = '--';
                    temperatureElement.textContent = '--';
                    iconElement.src = '';
                    timeElement.textContent = '';
                });
        }

        function updateClouds(temp) {
            // Show clouds based on temperature
            clouds.forEach(cloud => cloud.style.display = 'none'); // Hide all clouds first

            if (temp < 10) {
                clouds[0].style.display = 'block'; // Show one cloud for very cold weather
                clouds[1].style.display = 'block';
                clouds[2].style.display = 'block';
                clouds[3].style.display = 'block';
                clouds[4].style.display = 'block';
            } else if (temp >= 10 && temp < 20) {
                clouds[0].style.display = 'block'; // Show fewer clouds for cool weather
                clouds[1].style.display = 'block';
                clouds[2].style.display = 'block';
            } else if (temp >= 20 && temp < 30) {
                clouds[0].style.display = 'block'; // Show only one cloud for warm weather
            }
        }

        // Default to Paris weather on page load
        document.addEventListener('DOMContentLoaded', function () {
            fetchWeatherData('Paris');
        });
    </script>
</body>

</html>
