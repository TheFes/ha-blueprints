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

### Weather entity

|Setting|Type|Description|
|---|---|---|
|Weather Entity|Entity|The weather entity which will be used to get the forecasts from. This best functionality it should be able to provide `hourly` and `daily` forecasts, and the `daily` forecast should include the current day (otherwise you won't be able to ask for today's weather).|

### Trigger settings

|Setting|Type|Description|
|---|---|---|
|Trigger for weather forecast|Text|The sentences which will trigger the automation. `{phrase}` refers to the moment from which you want the forecast, e.g. `Tomorrow` or `afternoon`|

#### Trigger sentence syntax

||Description|Example
|---|---|---|
|`[` `]`|An optional part, the sentence will trigger with, or without it|`what is the [current] weather`|
|`\|`|Indicates `or`. Can also be used in an optional part|`what is the weather [currently\|right now]`|
|`(` `)`|Can be used in combination with the `\|` to indicate at least one of the options is required|`(what's\|what is) the weather`|

Capitalization is ignored when checking a (voice) command with a trigger sentence. Double spaces (due to optional parts) are trimmed.

### Settings for the daily forecast / Settings for the forecast of a part of a day

These settings can be use to set which terms can be used to request a forecast. Multiple terms can be entered for a single parameter.

#### Note on the terms for part of a day

When checking for the terms which are part of the day, they will also be checked against the values set for `Tomorrow`. If there is a match for one of the terms for `Tomorrow`, a day will be added.
So `What is the weather for this morning` will give the weather for the morning of today, and `What is the weather for tomorrow morning` will give the forecast for the morning of the next day.

### Settings for the responses

First part of this section set how the state of the weather entity will be used in the response. So for example the state `windy-cloudy` will be `windy and cloudy`.

#### Variables

These variables can be used in the response

variable|type|description
---|---|---
condition|text|The (most frequent occuring) weather condition
cloud_coverage|number|The (highest) percentage of cloud coverage
temperature|number|The (highest) temperature
dew_point|number|The (highest) dew point
pressure|number|The (highest) pressure
humidity|number|The (highest) humidity
templow|number|The low temperature for the day forecast (not provided in day part forecast)
wind_gust_speed|number|The wind gust speed (not provided in day part forecast)
apparent_temperature|number|The (highest) apparant temperature
uv_index|number|The (hightest) uv index
precipitation|number|The (total) precipitation
precipitation_probability|number|The (highest) probability for rain
wind_speed|number|The (highest) wind speed
condition_translated|text|The translated condition using the blueprint settings
rain_warning_threshold|number|Set in the blueprint settings, can be used to not respond about rain when the probability is low
wind_phrase|text|Term to describe the wind as set in the blueprint settings
wind_threshold|number|Set in the blueprint settings, can be used to not respond about wind when the speed is low
wind_joke_threshold|number|Set in de blueprint, it is used in the default response template to issue a small joke about the wind
wind_warning_threshold|number|Set in the blueprint, can be used to issue a warning about the wind
