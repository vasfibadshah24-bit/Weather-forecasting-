<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>One-Click Weather</title>
    <style>
      body {
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
          Helvetica, Arial, sans-serif;
        color: #fff;
        background-color: #282c34;
        margin: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        text-align: center;
        transition: background-image 1s ease-in-out;
        background-size: cover;
      }
      .weather-container {
        padding: 40px;
        background: rgba(0, 0, 0, 0.4);
        border-radius: 12px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
      }
      h2 {
        font-size: 2em;
        margin: 0 0 10px 0;
        font-weight: 600;
      }
      h3 {
        font-size: 1.5em;
        margin: 0 0 5px 0;
      }
      p {
        margin: 5px 0;
        font-size: 1.2em;
      }
      .weather-icon {
        width: 100px;
        height: 100px;
      }
    </style>
  </head>
  <body>
    <div class="weather-container">
      <h2>Loading Weather...</h2>
      <div id="weather-info" style="display: none">
        <h3 id="location"></h3>
        <img id="weather-icon" class="weather-icon" alt="Weather icon" />
        <p id="description"></p>
        <p id="temperature"></p>
      </div>
    </div>

    <script>
      const API_KEY = "YOUR_API_KEY"; // Get your key from openweathermap.org
      const DEFAULT_CITY = "Hyderabad"; // Set your default city
      const weatherInfo = document.getElementById("weather-info");
      const loadingHeader = document.querySelector("h2");
      const body = document.body;

      // Map weather descriptions to background images (URLs)
      const backgroundImages = {
        Clear: "https://images.unsplash.com/photo-1601297183305-6df142704ea2",
        Clouds: "https://images.unsplash.com/photo-1517685352821-92cf88aee5a5",
        Rain: "https://images.unsplash.com/photo-1534274988757-b889b704a081",
        Drizzle: "https://images.unsplash.com/photo-1534274988757-b889b704a081",
        Thunderstorm:
          "https://images.unsplash.com/photo-1493246540650-7a2176be1508",
        Snow: "https://images.unsplash.com/photo-1542601098-8fc114cd6875",
        Mist: "https://images.unsplash.com/photo-1525695232952-6b3a0e633de0",
        default:
          "https://images.unsplash.com/photo-1580193813131-4824d547f8c7",
      };

      async function getWeatherData(city) {
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`;
        try {
          const response = await fetch(url);
          if (!response.ok) {
            throw new Error("City not found or API error.");
          }
          const data = await response.json();
          displayWeather(data);
        } catch (error) {
          console.error("Error fetching weather data:", error);
          loadingHeader.innerText = "Error loading weather data.";
        }
      }

      function displayWeather(data) {
        const { name, main, weather } = data;
        const weatherDescription = weather[0].main;
        const iconCode = weather[0].icon;
        const iconUrl = `http://openweathermap.org/img/wn/${iconCode}@2x.png`;

        document.getElementById("location").innerText = name;
        document.getElementById(
          "temperature"
        ).innerText = `${Math.round(main.temp)}°C`;
        document.getElementById("description").innerText =
          weather[0].description;
        document.getElementById("weather-icon").src = iconUrl;

        loadingHeader.style.display = "none";
        weatherInfo.style.display = "block";

        // Set dynamic background image
        const backgroundImage =
          backgroundImages[weatherDescription] || backgroundImages.default;
        body.style.backgroundImage = `url('${backgroundImage}')`;
      }

      // Fetch weather data when the page loads
      window.onload = () => {
        getWeatherData(DEFAULT_CITY);
      };
    </script>
  </body>
</html>
