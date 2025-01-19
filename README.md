# PRODIGY_WD_05
This repository contains the code for a Weather App that allows users to fetch real-time weather information based on their current location or an inputted city name. The app displays essential weather details such as temperature, weather description, and current date &amp; time.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f0f4f8;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            color: #333;
        }
        .weather-container {
            background: #fff;
            border-radius: 10px;
            padding: 30px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            width: 350px;
            text-align: center;
        }
        h1 {
            font-size: 2rem;
            margin-bottom: 15px;
            color: #333;
        }
        .location {
            font-size: 1.5rem;
            margin-bottom: 10px;
            color: #555;
        }
        .weather-info {
            margin-top: 20px;
            font-size: 1.2rem;
        }
        .temp {
            font-size: 2.5rem;
            font-weight: bold;
            margin: 10px 0;
        }
        .description {
            font-size: 1.2rem;
            color: #888;
        }
        input[type="text"] {
            padding: 8px;
            border-radius: 5px;
            border: 1px solid #ccc;
            width: 80%;
            margin-bottom: 20px;
        }
        button {
            padding: 10px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        .date-time {
            font-size: 1rem;
            margin-top: 10px;
            color: #555;
        }
    </style>
</head>
<body>
    <div class="weather-container">
        <h1>Weather App</h1>
        <input type="text" id="location-input" placeholder="Enter city name...">
        <button onclick="getWeatherData()">Get Weather</button>
        
        <div id="weather-details">
            <div class="location" id="location"></div>
            <div class="date-time" id="date-time"></div>
            <div class="weather-info">
                <div class="temp" id="temp"></div>
                <div class="description" id="description"></div>
            </div>
        </div>
    </div>

    <script>
        const apiKey = 'YOUR_API_KEY';  // Replace with your OpenWeatherMap API key
        
        // Function to format the date and time
        function formatDateTime(date) {
            const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const time = date.toLocaleString('en-US', options);
            return time;
        }

        // Function to get weather data
        function getWeatherData() {
            const location = document.getElementById('location-input').value;
            const weatherDetails = document.getElementById('weather-details');
            
            if (location === '') {
                alert('Please enter a city name');
                return;
            }
            
            // Show loading state
            weatherDetails.innerHTML = 'Loading...';

            // Fetching weather data from OpenWeatherMap API
            fetch(`https://api.openweathermap.org/data/2.5/weather?q=${location}&appid=${apiKey}&units=metric`)
                .then(response => response.json())
                .then(data => {
                    if (data.cod !== 200) {
                        alert('City not found!');
                        weatherDetails.innerHTML = '';
                        return;
                    }

                    // Get current date and time
                    const date = new Date();
                    const formattedDate = formatDateTime(date);

                    // Extract weather information
                    const cityName = data.name;
                    const temp = data.main.temp;
                    const description = data.weather[0].description;
                    const country = data.sys.country;

                    // Update UI with weather data
                    document.getElementById('location').innerHTML = `${cityName}, ${country}`;
                    document.getElementById('temp').innerHTML = `${temp}°C`;
                    document.getElementById('description').innerHTML = description;
                    document.getElementById('date-time').innerHTML = formattedDate;
                })
                .catch(error => {
                    alert('Error fetching weather data. Please try again.');
                    weatherDetails.innerHTML = '';
                });
        }

        // Optional: Get weather based on user location (Geolocation)
        window.onload = function() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function(position) {
                    const lat = position.coords.latitude;
                    const lon = position.coords.longitude;

                    fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`)
                        .then(response => response.json())
                        .then(data => {
                            if (data.cod === 200) {
                                const cityName = data.name;
                                const temp = data.main.temp;
                                const description = data.weather[0].description;
                                const country = data.sys.country;

                                document.getElementById('location').innerHTML = `${cityName}, ${country}`;
                                document.getElementById('temp').innerHTML = `${temp}°C`;
                                document.getElementById('description').innerHTML = description;
                                document.getElementById('date-time').innerHTML = formatDateTime(new Date());
                            }
                        });
                });
            }
        };
    </script>
</body>
</html>
