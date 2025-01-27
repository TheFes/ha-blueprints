![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Weather Forecasts - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F1_voice_weather_forecast_local.yaml)

Using this blueprint you can create an automation to request the weather forecast. The automation runs totally local, no LLM is required to use this automation.

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* For full days you can only ask forecasts up to one weak ahead and for a period of one full day. For partial days only `night`, `morning`, `afternoon` and `evening` are possible, no other (subset of) hours.
* The response is predefined, and therefor will follow the same format every time you request the weather. 
* In case you want to use it in another language as English, you need to provide translations for a large number of settings.

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Provide a weather entity which will be used for the forecasts
4. (optionally) change or add the triggers provided in the blueprint settings
5. (optionally) change the phrases used to refer to the days and day parts
6. (optionally) change the response given by Assist

## Detailed instructions for blueprint settings

![Image](/images/Under_construction_graphic.gif?raw=true)