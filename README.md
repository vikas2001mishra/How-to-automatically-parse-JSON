# How-to-automatically-parse-JSON

import json
from typing import List

class Coord:
    def __init__(self, lon: float, lat: float):
        self.lon = lon
        self.lat = lat

class Weather:
    def __init__(self, id: int, main: str, description: str, icon: str):
        self.id = id
        self.main = main
        self.description = description
        self.icon = icon

class Main:
    def __init__(self, temp: float, feels_like: float, temp_min: float, temp_max: float, pressure: int, humidity: int, sea_level: int, grnd_level: int):
        self.temp = temp
        self.feels_like = feels_like
        self.temp_min = temp_min
        self.temp_max = temp_max
        self.pressure = pressure
        self.humidity = humidity
        self.sea_level = sea_level
        self.grnd_level = grnd_level

class Wind:
    def __init__(self, speed: float, deg: int, gust: float):
        self.speed = speed
        self.deg = deg
        self.gust = gust

class Clouds:
    def __init__(self, all: int):
        self.all = all

class Sys:
    def __init__(self, country: str, sunrise: int, sunset: int):
        self.country = country
        self.sunrise = sunrise
        self.sunset = sunset

class WeatherData:
    def __init__(self, coord: Coord, weather: List[Weather], base: str, main: Main, visibility: int, wind: Wind, clouds: Clouds, dt: int, sys: Sys, timezone: int, id: int, name: str, cod: int):
        self.coord = coord
        self.weather = weather
        self.base = base
        self.main = main
        self.visibility = visibility
        self.wind = wind
        self.clouds = clouds
        self.dt = dt
        self.sys = sys
        self.timezone = timezone
        self.id = id
        self.name = name
        self.cod = cod

def map_json_to_weather_data(json_data):  #Map JSON Data: Create a helper function to map the JSON data to the defined classes.
    coord = Coord(**json_data['coord'])   #It allows you to pass the key-value pairs of a dictionary as arguments to a function or a class constructor.
    weather = [Weather(**w) for w in json_data['weather']]
    main = Main(**json_data['main'])
    wind = Wind(**json_data['wind'])
    clouds = Clouds(**json_data['clouds'])
    sys = Sys(**json_data['sys'])
    weather_data = WeatherData(coord, weather, json_data['base'], main, json_data['visibility'], wind, clouds, json_data['dt'], sys, json_data['timezone'], json_data['id'], json_data['name'], json_data['cod'])
    return weather_data

# Load JSON data from a file
with open('Resources/WeatherData.json', 'r') as json_file:
    json_data = json.load(json_file)      #Load JSON Data: Use the json module to load the JSON data from a file.

# Map JSON data to WeatherData object
weather_data = map_json_to_weather_data(json_data)

# Convert temperature from Kelvin to Celsius
temperature_celsius = weather_data.main.temp - 273.15

# Accessing data
print(f"City: {weather_data.name}")
print(f"Country: {weather_data.sys.country}")
print(f"Wind Speed: {weather_data.wind.speed} m/s")
print(f"Temperature: {temperature_celsius:.2f}°C")
print(f"Weather: {weather_data.weather[0].main}")
print(f"Description: {weather_data.weather[0].description}")
print(f"Coordinates: ({weather_data.coord.lon}, {weather_data.coord.lat})")
