<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Weather Forecasts - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F3_voice_weather_forecast_full_llm.yaml)

Using this blueprint you can create a script to request the weather forecast. It allows you to ask any weather related question, so not only the weather forecast, but also if an umbrella is required if you go outside. You can ask for full days, periods of a day (e.g. morning) or specific hour ranges.

> [!TIP]
> There is also a [version 2](3_voice_weather_forecast_full_llm_v2.md) of this script, with a single `query` field and an AI Task to determine the period.

## Limitations of this script

* You need to have an LLM integration set up and use it in your voice pipeline
* The LLM integration needs access to your house (Assist), otherwise it can't access the script as a tool

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. Select the weather entity to be used for the forecasts
4. (optionally) adjust the prompts which are used to instruct the LLM how to use the script fields
5. Make sure to provide a clear description for the script (example below)
6. Save the script, and expose it to Assist

### Example for script description:

> Fetches the weather forecast for either a part of a day, or one or more full days. In case the weather for the weekend is requested, this means Saturday and Sunday

## What the LLM receives

The script returns the start and end of the requested period, the forecast type, the units of the weather entity and the forecast data. When the period contains the current time, the current conditions of the weather entity are added.

With `Calculate averages` enabled (default) the forecast data is a summary of the period: the minimum and maximum temperature, the total precipitation, the highest precipitation probability, wind speed and UV index, the average wind direction, the most frequent condition and all conditions in the period, and the averages of the other values. With the setting disabled, the forecast for each hour or day is sent, with the times converted to your local time zone.

When something is wrong (no date provided, the period is in the past, or there is no forecast for the requested period) the LLM receives an `error` explaining what went wrong, including the period for which a forecast is available.

When the weather provider has no hourly forecast, the daily forecast for the days which contain the requested hours is used, and the LLM is told so.

## Usage

You can request the forecast in any way you can think of, using any language. Unless set otherwise in the LLM
configuration, the response will be in the same language as the command.

You can refer to a single day, multiple days (e.g. next weekend) a part of the day (e.g. next morning) or one or more hours
(e.g. from 5 to 9).

You can ask for specific weather conditions, for example, will it rain tomorrow. Or should I take an umbrella if I 
go out this evening.


## Examples

```
Give me the weather forecast for this evening
Do I need to apply sunscreen if I go out tomorrow morning
Will it rain at 9 this evening
What's the weather for this weekend
```


