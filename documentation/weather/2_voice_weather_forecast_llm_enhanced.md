![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Weather Forecasts - LLM Enhanced

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F2_voice_weather_forecast_llm_enhanced.yaml)

Using this blueprint you can create an automation to ask weather related questions. The question is caught by local trigger sentences, so the voice pipeline can use the local Home Assistant conversation agent (or an LLM with local preference), and the LLM doesn't need access to your house.

An [AI Task](https://www.home-assistant.io/integrations/ai_task/) (`ai_task.generate_data`) is used twice:

1. To determine the requested period from your question. This uses structured output, so the AI can only return a forecast type, a date, a time and a length.
2. To answer your question, based on the forecast data for that period. The answer is in the language of the question.

The forecast data is retrieved and summarized by Home Assistant, so the AI only receives the data for the requested period.

## Requirements

* Home Assistant 2025.8 or newer
* An AI Task entity (from for example Google Generative AI, OpenAI or Anthropic). You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant
* A weather entity, preferably with an hourly and a daily forecast

## Limitations of this automation

* The sentence still has to match one of the trigger sentences. Within those sentences you can ask anything
* The trigger sentences and the error response need to be translated when you use another language than English
* Each question uses two AI Task calls, so the response takes a bit longer than with the local blueprint

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Select the weather entity to be used for the forecasts
4. (optionally) select the AI Task entity, if you don't want to use the preferred one
5. (optionally) change or translate the trigger sentences
6. (optionally) adjust the instructions for the AI

## Blueprint settings

### Weather settings

|Setting|Type|Description|
|---|---|---|
|Weather entity|Entity|The weather entity used for the forecasts|
|Include the forecast per hour or day|Boolean|Besides the summary of the period, also send the forecast for each hour or day (up to 24 items) to the AI. This allows answers like "it will start raining around 4", but uses more tokens. Default `true`|

### AI settings

|Setting|Type|Description|
|---|---|---|
|AI Task entity|Entity|The AI Task entity to use. Leave empty to use the preferred AI Task entity|
|Instructions to determine the period|Text|How the AI should translate the question into a period. `{{ trigger.sentence }}` is the question|
|Instructions for the response|Text|How the AI should answer. The question, the period and the weather data are added automatically. `{{ trigger.sentence }}` and `{{ period_description }}` can be used|

### Trigger and responses

|Setting|Type|Description|
|---|---|---|
|Conversation trigger sentences|Text|The sentences which trigger the automation. `{query}` is a wildcard, the full sentence is always sent to the AI|
|Error response|Text|Used when the AI Task fails, or the weather provider returned no data|

When there is no forecast for the requested period (for example a date two weeks ahead), the AI tells you which period it does have a forecast for.

### What the AI receives

For the requested period the AI receives the start and end, the units of the weather entity, a summary and (optionally) the forecast for each hour or day. When the period contains the current time, the current conditions of the weather entity are added as well.

|Summary key|Description|
|---|---|
|`condition_most_frequent`, `conditions_in_period`|The most frequent condition, and all conditions in the period|
|`temperature`, `apparent_temperature`, `dew_point`|Minimum and maximum|
|`templow_min`|The lowest low temperature (daily forecasts)|
|`precipitation_total`|Total precipitation|
|`precipitation_probability_max`, `wind_speed_max`, `wind_gust_speed_max`, `uv_index_max`|The highest values|
|`wind_bearing_average`|The average wind direction (circular average)|
|Other numeric values|`<key>_average`|

## Translation

### Dutch

Paste these trigger sentences in the automation (or replace them in the UI):

```yaml
trigger:
  - "(wat|hoe) (is|wordt) het weer [{query}]"
  - "wat voor weer (is|wordt) het [{query}]"
  - "wat is de [weers]verwachting [{query}]"
  - "(gaat|zal) het [{query}] (regenen|sneeuwen|vriezen|waaien)"
  - "wordt het [{query}] (zonnig|koud|warm|droog|mooi [weer])"
  - "(heb ik|moet ik) [{query}] (een paraplu|zonnebrand|een jas) (nodig|mee[nemen]|op[doen]|aan[doen])"
error_response: Sorry, er ging iets mis bij het ophalen van de weersverwachting
```

The instructions for the AI can stay in English, the answer will be in the language of the question.

## Examples

- *What's it like right now*

  Response: *It is currently partly cloudy with no rain expected. The temperature is 8 degrees, though it feels like 3 degrees because of the wind.*

- *When will it start raining tomorrow*

  Response: *It will start raining tomorrow at six in the morning. You should definitely bring an umbrella as it will be a rainy day with strong winds throughout the evening.*

- *Do I need an umbrella tomorrow evening*

  Response: *Yes, you should bring an umbrella for tomorrow evening as it will be rainy. It will also be quite windy, with gusts up to forty-three kilometers per hour.*

- *Wat voor weer wordt het op oudejaarsavond*

  Response: *Ik heb helaas geen weersverwachting voor oudejaarsavond. Ik kan je wel voorzien van een weersverwachting voor de periode tussen nu en 23 november.*

## Differences with the other weather blueprints

|Topic|1: Local|2: LLM Enhanced|3: Full LLM script|
|---|---|---|---|
|LLM required|No|AI Task only|LLM conversation agent with access to your house|
|Sentences|Strict trigger sentences, with fixed phrases for the period|Trigger sentences, the period can be anything|Anything|
|Periods|Today up to a week ahead, or night, morning, afternoon, evening (optionally with a day)|Any period, hours or days|Any period, hours or days|
|Response|Fixed template|Generated by the AI, in the language of the question|Generated by the LLM|
|Translations needed|Everything|Trigger sentences and error response|None|
