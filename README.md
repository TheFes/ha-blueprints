![Version](https://img.shields.io/github/v/release/TheFes/ha-blueprints)
[![Buy me a coffe](https://img.shields.io/static/v1.svg?label=%20&message=Buy%20me%20a%20coffee&color=6f4e37&logo=buy%20me%20a%20coffee&logoColor=white)](https://www.buymeacoffee.com/TheFes)
[![Paypal](https://img.shields.io/badge/PayPal-00457C?&color=00457c&logo=paypal&logoColor=white)](https://www.paypal.com/paypalme/thefes)

# Home Assistant Voice Blueprints
![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

## Under construction

This Repository is still under contstruction. The contents in general, but also the blueprints might change quite frequently. I'll try not to introduce any backwards incompatible changes.

## About this repository

On this respository I will put my blueprints which are created to help issuing advanced (voice) commands using Assist. 
There are basically 5 ways to configure a voice pipeline in Home Assistant:

|#|LLM|Home Control (Assist) enabled|Local preference enabled|Description|
|---|---|---|---|---|
|1|❌|NA|NA|Local only|
|2|✅|❌|❌|LLM as voice agent, no home control, no local preference|
|3|✅|❌|✅|LLM as voice agent, no home control, with local preference|
|4|✅|✅|❌|LLM as voice agent, with home control, no local preference|
|5|✅|✅|✅|LLM as voice agent, with home control, with local preference|

On this repository different blueprints will be placed with the same goal. The will differ in which op the above mentioned configurations are supported. Based on that, also other aspects will differ.

## Blueprint options

There are 3 options for the blueprints
* Option 1: Local automation
* Option 2: LLM Enhanced automation
* Option 3: Full LLM script

|Description|Option 1|Option 2|Option 3|
|---|---|---|---|
|Supported voice configurations|1, 2, 4|1, 3, 5|3, 4, 5|
|Fully local, no LLM required|✅|❌|❌|
|No home control for LLM required|✅|✅|❌|
|No strict voice commands|❌|✅|✅|
|Complete freedom in voice commands|❌|❌|✅|
|Variance in responses for each request|❌|✅|✅|
|Works without need for translations|❌|❌|✅|

The groundwork for option 2 was done by [JLo](<https://github.com/jlpouffier>) in his [blog post](<https://blog.jlpouffier.fr/chatgpt-powered-music-search-engine-on-a-local-voice-assistant/>) on GPT-powered music search. This blog post was a big inspiration for the LLM enhanced automation, and also enabled the full LLM script. So big thanks to JLo!

## The Blueprints

### Weather Forecasts

Using these blueprints you can ask for the weather forecast from a weather entity in your system.

|Option|Import Button|
|---|---|
|[1: Local](/documentation/weather/1_voice_weather_forecast_local.md)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F1_voice_weather_forecast_local.yaml)|
|2: LLM Enhanced|To be created|
|[3: Full LLM](/documentation/weather/3_voice_weather_forecast_full_llm.md)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F3_voice_weather_forecast_full_llm.yaml)|

### Calendar

Using these blueprints you can ask for the calendar entries from one or more calendar entities in your system.

|Option|Import Button|
|---|---|
|1: Local|To be created|
|2: LLM Enhanced|To be created|
|[3: Full LLM](/documentation/calendar/3_voice_calendar_entries_full_llm.md)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F3_voice_calendar_entries_full_llm.yaml)|

### Music Assistant

For completeness I also mention these blueprints here. They are located on the Music Assistant Voice support [repository](<https://github.com/music-assistant/voice-support>).
I'm also a maintainer of that repository and did a lot of work on those blueprints together with some other guys.

Using these bleuprints you can issue voice commands to play media on your Music Assistant media players.

|Option|Import Button|
|---|---|
|[1: Local](https://github.com/music-assistant/voice-support#option-1-local-assistant-blueprint)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Flocal-assist-blueprint%2Fmass_assist_blueprint_en.yaml)|
|[2: LLM Enhanced](https://github.com/music-assistant/voice-support#option-2-local-assist-enhanced-by-an-llm-integration-like-open-ai-conversation-chatgpt-or-google-generative-ai-gemini)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Fllm-enhanced-local-assist-blueprint%2Fmass_llm_enhanced_assist_blueprint_en.yaml)|
|[3: Full LLM](https://github.com/music-assistant/voice-support#option-3-script-which-can-be-used-as-a-tool-by-an-llm-integration-like-open-ai-conversation-chatgpt-or-google-generative-ai-gemini)|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Fllm-script-blueprint%2Fllm_voice_script.yaml)|

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)
