import requests
import pandas as pd
import matplotlib.pyplot as plt

# Function to fetch weather data from OpenWeatherMap API
def fetch_weather_data(city, api_key):
    url = f"http://api.openweathermap.org/data/2.5/weather?q={city}&appid={api_key}&units=metric"
    response = requests.get(url)
    
    if response.status_code == 200:
        data = response.json()
        return {
            'city': data['name'],
            'temperature': data['main']['temp'],
            'humidity': data['main']['humidity'],
            'weather': data['weather'][0]['description']
        }
    else:
        print("Error fetching data:", response.status_code)
        return None

# Function to visualize temperature
def visualize_weather_data(weather_data):
    df = pd.DataFrame(weather_data)

    # Create a bar chart for temperatures
    plt.bar(df['city'], df['temperature'], color='blue')
    plt.title('Current Temperature in City')
    plt.xlabel('City')
    plt.ylabel('Temperature (°C)')
    plt.ylim(0, max(df['temperature']) + 5)
    plt.show()

def main():
    api_key = 'YOUR_API_KEY'  # Replace with your OpenWeatherMap API key
    cities = ['London', 'New York', 'Tokyo', 'Mumbai']  # Example cities

    weather_data = []
    for city in cities:
        data = fetch_weather_data(city, api_key)
        if data:
            weather_data.append(data)

    if weather_data:
        visualize_weather_data(weather_data)

if __name__ == '__main__':
    main()
