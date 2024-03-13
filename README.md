# weather-wizard-website
"weather wizard" is created using HTML and Javascript,CSS which creates a webpage  that allows users to input a city name, retrieve weather information using the OpenWeatherMap API, and display the weather details along with other features.
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Weather Report</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Montserrat', sans-serif;
      text-align: center;
      background-color: #f2f2f2;
      transition: background-color 1.5s ease;
    }
    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 20px;
    }
    .card {
      background-color: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      margin-bottom: 30px;
    }
    .weather-icon {
      width: 80px;
      height: 80px;
    }
    .clock {
      font-size: 64px;
      font-weight: bold;
      color: #333;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    }
    .quote {
      font-style: italic;
      color: #555;
      font-size: 18px;
    }
    h1, h2 {
      color: #333;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
    }
    input, button {
      padding: 10px;
      border-radius: 5px;
      border: 1px solid #ccc;
      font-size: 16px;
    }
    button {
      background-color: #4CAF50;
      color: white;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #235c24;
    }
  </style>
</head>
<body>
  <h1>Weather Report</h1>
  <div class="container">
    <div class="card">
      <h2>Get Weather Report</h2>
      <input type="text" id="city-input" placeholder="Enter city name">
      <button id="search-btn">Get Weather</button>
      <h2 id="city-name">-</h2>
      <img id="weather-icon" class="weather-icon" src="" alt="Weather Icon">
      <p id="temperature">Temperature: -</p>
      <p id="description">Description: -</p>
      <p id="humidity">Humidity: -</p>
      <p id="wind">Wind Speed: -</p>
      <button id="toggle-unit-btn">Toggle Unit</button>
      <p>Brought to you by Spam Bots</p>
    </div>
    <div class="card">
      <h1>Your local time</h1>
      <div class="clock" id="clock"></div>
    </div>
    <div class="card">
      <p class="quote" id="quote"></p>
    </div>
  </div>

  <script>
    // Replace 'YOUR_API_KEY' with your actual API key from OpenWeatherMap
    const API_KEY = '9d3eed3a8282738700391ba8030632cd';
    let isCelsius = true; // Default temperature unit is Celsius

    // Array of motivational quotes
    const quotes = [
      "Believe you can and you're halfway there. - Theodore Roosevelt",
      "The future belongs to those who believe in the beauty of their dreams. - Eleanor Roosevelt",
      "Success is not final, failure is not fatal: It is the courage to continue that counts. - Winston Churchill",
      "The only way to do great work is to love what you do. - Steve Jobs",
      "Happiness is not something ready-made. It comes from your own actions. - Dalai Lama",
      "umbrella helps you when it's sunny and rainy! - Team Spam Bots ",
      ":D"
    ];

    // Function to fetch weather data from the API
    async function fetchWeatherData(city) {
      const unit = isCelsius ? 'metric' : 'imperial';
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=${unit}`;

      try {
        const response = await fetch(apiUrl);
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error fetching weather data:', error);
        return null;
      }
    }

    // Function to update the weather report
    function updateWeatherReport(weatherData) {
      const cityNameElement = document.getElementById('city-name');
      const weatherIconElement = document.getElementById('weather-icon');
      const temperatureElement = document.getElementById('temperature');
      const descriptionElement = document.getElementById('description');
      const humidityElement = document.getElementById('humidity');
      const windElement = document.getElementById('wind');

      if (!weatherData) {
        cityNameElement.textContent = 'Failed to fetch weather data.';
        return;
      }

      const { name, weather, main, wind } = weatherData;
      const { description, icon } = weather[0];
      const { temp, humidity } = main;

      cityNameElement.textContent = name;
      weatherIconElement.src = `https://openweathermap.org/img/w/${icon}.png`;
      temperatureElement.textContent = `Temperature: ${temp}°${isCelsius ? 'C' : 'F'}`;
      descriptionElement.textContent = `Description: ${description}`;
      humidityElement.textContent = `Humidity: ${humidity}%`;
      windElement.textContent = `Wind Speed: ${wind.speed} ${isCelsius ? 'm/s' : 'mi/hr'}`; //${wind.speed} m/s`;

      // Change background color based on temperature
      changeBackgroundColor(temp);
    }

    // Function to change background color based on temperature
    function changeBackgroundColor(temperature) {
      const body = document.querySelector('body');
      if (isCelsius) {
        if (temperature >= 30) {
          body.style.backgroundColor = '#f7e0b5'; // Red for hot weather
        } else if (temperature > 10 && temperature < 30) {
          body.style.backgroundColor = '#C9D9C3';
        } else if (temperature <= 10) {
          body.style.backgroundColor = '#a7c5eb'; // Blue for cold weather
        } else {
          body.style.backgroundColor = '#f2f2f2'; // Default background color
        }
      } else {
        if (temperature >= 86) {
          body.style.backgroundColor = '#f7e0b5'; // Red for hot weather
        } else if (temperature <= 50) {
          body.style.backgroundColor = '#99ccff'; // Blue for cold weather
        }
      }
    }

    // Event listener for the search button
    const searchBtn = document.getElementById('search-btn');
    const cityInput = document.getElementById('city-input');

    searchBtn.addEventListener('click', () => {
      const city = cityInput.value.trim();
      if (city) {
        fetchWeatherData(city)
          .then(weatherData => updateWeatherReport(weatherData))
          .catch(error => console.error('Error:', error));
      }
    });

    // Event listener for toggle unit button
    const toggleUnitBtn = document.getElementById('toggle-unit-btn');
    toggleUnitBtn.addEventListener('click', () => {
      isCelsius = !isCelsius;
      toggleUnitBtn.textContent = isCelsius ? 'Toggle to Fahrenheit' : 'Toggle to Celsius';
      // Re-fetch weather data with updated unit
      const city = cityInput.value.trim();
      if (city) {
        fetchWeatherData(city)
          .then(weatherData => updateWeatherReport(weatherData))
          .catch(error => console.error('Error:', error));
      }
    });

    // Function to update the clock
    function updateClock() {
      const clockElement = document.getElementById('clock');
      const now = new Date();
      const hours = now.getHours().toString().padStart(2, '0');
      const minutes = now.getMinutes().toString().padStart(2, '0');
      const seconds = now.getSeconds().toString().padStart(2, '0');
      clockElement.textContent = `${hours}:${minutes}:${seconds}`;
    }

    // Function to display a random motivational quote
    function displayQuote() {
      const quoteElement = document.getElementById('quote');
      const randomIndex = Math.floor(Math.random() * quotes.length);
      quoteElement.textContent = quotes[randomIndex];
    }

    // Update the clock every second
    setInterval(updateClock, 1000);

    // Display a random quote when the page loads
    displayQuote();

    // Change the quote every 30 seconds
    setInterval(displayQuote, 25000);
  </script>
</body>
</html>
