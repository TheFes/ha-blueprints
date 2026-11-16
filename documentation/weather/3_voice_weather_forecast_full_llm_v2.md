<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Weather Forecasts - Full LLM - version 2

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F3_voice_weather_forecast_full_llm_v2.yaml)

Version 2 of the [Full LLM script](3_voice_weather_forecast_full_llm.md). The LLM in your voice pipeline only passes the question in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) with structured output determines the requested period, Home Assistant collects and summarizes the forecast, and the result is returned to the LLM, which answers the question.

It is added next to version 1, because the fields of the script changed. Version 1 keeps working.

## Requirements

* Home Assistant 2025.8 or newer
* An LLM conversation agent in your voice pipeline which is allowed to control your house (so it can call the script)
* An AI Task entity. This can be the same integration as your conversation agent, but you can also use a smaller and faster model

## How to use the blueprint

1. Import the blueprint using the button above
2. Create a script using the blueprint
3. Select the weather entity, and optionally the AI Task entity (leave empty to use the preferred AI Task entity)
4. Give the script a clear description (example below)
5. Save the script, and expose it to Assist

### Example for script description:

> Gets the weather forecast and current weather conditions. Use this tool for every question about the weather, also for questions like "do I need an umbrella". Pass the complete question of the user in the query field, in the words of the user.

## Blueprint settings

|Setting|Type|Description|
|---|---|---|
|Weather entity|Entity|The weather entity used for the forecasts|
|Include the forecast per hour or day|Boolean|Besides the summary of the period, also return the forecast for each hour or day (up to 24 items). Default `true`|
|AI Task entity|Entity|The AI Task entity used to determine the period. Leave empty to use the preferred AI Task entity|
|Instructions to determine the period|Text|How the AI Task should translate the question into a period. `{{ query }}` is the question|
|Query field description|Text|The description of the `query` field, which tells the LLM what to pass to the script|

## What the LLM receives

The requested period (as described by the AI Task, plus start and end), the forecast type, the units of the weather entity, a summary for the period and (optionally) the forecast for each hour or day, with times in your local time zone. When the period contains the current time, the current conditions of the weather entity are added as well. See the [LLM Enhanced documentation](2_voice_weather_forecast_llm_enhanced.md#what-the-ai-receives) for the keys in the summary.

When there is no forecast for the requested period, the result contains an `error` and the period for which a forecast is available, so the LLM can tell you.

## What is different compared to version 1

|Topic|Version 1|Version 2|
|---|---|---|
|Fields|Four fields (`time_period_type`, `time_period_length`, `start_date`, `start_time`), the LLM has to work out the dates and times itself|One field: `query`. The LLM only passes the question|
|Determining the period|By the LLM in your voice pipeline, guided by four prompt inputs|By an AI Task with structured output, which gets the current date and time explicitly. One set of instructions replaces the four prompts|
|Result|Summary or the full forecast (`Calculate averages`)|Summary, and optionally the forecast per hour or day (max 24 items)|
|Questions like "when will it start raining tomorrow"|Depends on the LLM choosing hourly data|The instructions tell the AI Task to use the hourly forecast for that day|
|Speed|One LLM call (by the voice pipeline)|One extra AI Task call, which can be a small, fast model|

### Removed and renamed inputs

|Version 1|Version 2|
|---|---|
|`time_period_type_prompt`, `time_period_length_prompt`, `date_prompt`, `time_prompt`|`llm_instructions` (for the AI Task) and `query_prompt` (the description of the `query` field)|
|`calc_average`|`include_details` (the summary is always included)|
|`weather_entity`|Unchanged|
|-|New: `ai_task_entity`|

### Switching from version 1

Create a new script from the version 2 blueprint, give it the description above, expose it to Assist and remove the exposure of the old script (or delete it). Don't keep both exposed, the LLM can't know which one to use.

## Examples

- *Moet ik morgenavond een paraplu meenemen?*

  The script returns the hourly forecast for tomorrow 18:00 - 24:00 (rain from 20:00, 53% chance at 23:00, gusts up to 43 km/h), and the LLM answers based on that.

- *What's the weather this weekend*

  The script returns the daily forecast for Saturday and Sunday.

- *Weather on Christmas day*

  The script returns an error that there is no forecast for that period, and that the forecast is available until 23 November.
