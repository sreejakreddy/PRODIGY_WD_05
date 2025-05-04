# PRODIGY_WD_05
Weather Dashboard 

<!DOCTYPE html>
<html lang="en">
<head>
  <title>Weather App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px;
    }

    h1 {
      color: #333;
    }

    .input-group {
      margin: 20px 0;
      display: flex;
      gap: 10px;
    }

    input, button {
      padding: 10px;
      font-size: 1rem;
    }

    .weather-card {
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
      width: 300px;
      text-align: center;
    }

    .error {
      color: red;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>🌦 Weather App</h1>

  <div class="input-group">
    <input type="text" id="cityInput" placeholder="Enter city name" />
    <button onclick="getWeatherByCity()">Search</button>
    <button onclick="getWeatherByLocation()">Use My Location</button>
  </div>

  <div id="output"></div>

  <script>
    const apiKey = 'YOUR_API_KEY_HERE';  // <--- Replace this

    function getWeather(url) {
      fetch(url)
        .then(response => response.json())
        .then(data => {
          if (data.cod !== 200) {
            document.getElementById('output').innerHTML =
              `<p class="error">❌ ${data.message}</p>`;
          } else {
            document.getElementById('output').innerHTML = `
              <div class="weather-card">
                <h2>${data.name}, ${data.sys.country}</h2>
                <p><strong>${data.weather[0].description}</strong></p>
                <p>🌡️ Temp: ${data.main.temp} °C</p>
                <p>💧 Humidity: ${data.main.humidity}%</p>
                <p>💨 Wind: ${data.wind.speed} m/s</p>
              </div>
            `;
          }
        })
        .catch(err => {
          document.getElementById('output').innerHTML =
            `<p class="error">❌ Error fetching data</p>`;
          console.error(err);
        });
    }

    function getWeatherByCity() {
      const city = document.getElementById('cityInput').value.trim();
      if (!city) {
        alert("Please enter a city name.");
        return;
      }

      const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;
      getWeather(url);
    }

    function getWeatherByLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          const { latitude, longitude } = position.coords;
          const url = `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${apiKey}&units=metric`;
          getWeather(url);
        }, () => {
          alert("Unable to get your location.");
        });
      } else {
        alert("Geolocation is not supported by your browser.");
      }
    }
  </script>

</body>
</html>
