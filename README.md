# weather-forecast
from requests import get
from datetime import datetime, timedelta
from json import loads
from pprint import pprint

KEY = '0c04e5ddacbc68d164d7af4e9a487982'

def get_city_id():
        with open('city.list.json') as f:
            data = loads(f.read())
        city = input('Which is the closest city to the place you are travelling to?' )
        city_id = False
        for item in data:
            if item['name'] == city:
                answer = input('Is this in ' + item['country'])
                if answer == 'y':
                        city_id = item['id']
                        break

        if not city_id:
                print('Sorry, that location is not available')
                exit()
                
        return city_id

def get_weather_data(city_id):
        weather_data = get('http://api.openweathermap.org/data/2.5/forecast?id={}&APPID={}'.format(city_id, KEY))

        return weather_data.json()

def get_arrival():
    today = datetime.now()
    max_day = today + timedelta(days = 4)
    print('What day of the month do you plan to arrive at your destination?')
    print(today.strftime('%d'), '-', max_day.strftime('%d'))
    day = input()
    print('What hour do you plan to arrive?')
    print('0 - 24')
    hour = int(input())
    hour = hour - hour % 3
    arrival = today.strftime('%Y') + '-' + today.strftime('%m') + '-' + day + ' ' + str(hour) + ':00:00'
    return arrival

def get_forecast(arrival, weather_data):
    for forecast in weather_data['list']:
        if forecast['dt_txt'] == arrival:
                return forecast

def get_readable_forecast(forecast):
    weather = {}
    #weather['cloudiness'] = forecast['clouds']['all']
    weather['temperature'] = float(forecast['main']['temp']-273)
    #weather['humidity'] = int(forecast['main']['humidity'])
    #if '3h' in forecast['rain']:
        #weather['rain'] = float(forecast['rain']['3h'])
    #else:
        #weather['rain'] = 0.0
    #weather['description'] = forecast['weather'][0]['description']
    #weather['wind'] = float(forecast['wind']['speed'])
    return weather
