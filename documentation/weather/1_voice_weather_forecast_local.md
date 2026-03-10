![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Weather Forecasts - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F1_voice_weather_forecast_local.yaml)

Using this blueprint you can create an automation to request the weather forecast. The automation runs totally local, no LLM is required to use this automation.

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* For full days you can only ask forecasts up to one weak ahead and for a period of one full day. For partial days only `night`, `morning`, `afternoon` and `evening` are possible, no other (subset of) hours.
* The response is predefined, and therefor will follow the same format every time you request the weather. 
* In case you want to use it in another language as English, you need to provide translations for a large number of settings. You can also find translations in the `translations` folder in this directory.

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

#### `response_current` vs `response_forecast`

- `response_current`: Used when the question is about the current weather. The variables below will contain data about today. Can be used together with the *Current state variables* to e.g. show current temperature **and** min/max for today.
- `response_forecast`: Used when the question is about future weather. The variables below will contain data about the requested `{{ phrase }}`.

#### Variables

These variables can be used in the response. For all numeric values, the `Value round` setting in the blueprint is used (defaults to 0 digits).

| Variable | Type | Description |
| :--- | :--- | :--- |
| `apparent_temperature` | number | The (highest) apparent temperature |
| `condition` | text | The (most frequent occurring) weather condition |
| `condition_translated` | text | The translated condition using the blueprint settings |
| `cloud_coverage` | number | The (highest or average) percentage of cloud coverage |
| `dew_point` | number | The (highest) dew point |
| `humidity` | number | The (highest) humidity |
| `pressure` | number | The (highest) pressure |
| `temperature` | number | The (highest) temperature |
| `templow` | number | The low temperature for the day forecast (not provided in day part forecast) |
| `precipitation` | number | The (total) precipitation |
| `precipitation_probability` | number | The (highest) probability for rain |
| `rain_warning_threshold` | number | Set in the blueprint settings, can be used to not respond about rain when the probability is low |
| `wind_speed` | number | The (highest) wind speed |
| `wind_direction` | text | The cardinal wind direction |
| `wind_phrase` | text | Term to describe the wind as set in the blueprint settings |
| `wind_gust_speed` | number | The wind gust speed (not provided in day part forecast) |
| `wind_threshold` | number | Set in the blueprint settings, can be used to not respond about wind when the speed is low |
| `wind_joke_threshold` | number | Set in the blueprint, it is used in the default response template to issue a small joke about the wind |
| `wind_warning_threshold` | number | Set in the blueprint, can be used to issue a warning about the wind |
| `uv_index` | number | The (highest) UV index |

#### Current state variables

These variables always provide the real-time state directly from the weather entity's state attributes at the time of execution.

| Variable | Type | Description |
| :--- | :--- | :--- |
| `current_condition` | text | The current weather condition |
| `current_condition_translated` | text | The translated current condition based on blueprint settings |
| `current_temperature` | number | The current temperature |
| `current_humidity` | number | The current humidity percentage |
| `current_dew_point` | number | The current dew point |
| `current_cloud_coverage` | number | The current percentage of cloud coverage |
| `current_uv_index` | number | The current UV index |
| `current_pressure` | number | The current air pressure |
| `current_apparent_temperature` | number | The current "feels like" temperature |
| `current_wind_gust_speed` | number | The current wind gust speed |
| `current_visibility` | number | The current visibility distance |
| `current_precipitation` | number | The current precipitation amount |
| `current_wind_bearing` | number | The current wind bearing in degrees |
| `current_wind_direction` | text | The current wind direction cardinal |
| `current_wind_speed` | number | The current wind speed |
| `current_wind_phrase` | text | Term to describe the current wind as set in the blueprint settings |