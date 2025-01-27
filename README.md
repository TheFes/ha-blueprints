![Version](https://img.shields.io/github/v/release/TheFes/ha-blueprints)
[![Buy me a coffe](https://img.shields.io/static/v1.svg?label=%20&message=Buy%20me%20a%20coffee&color=6f4e37&logo=buy%20me%20a%20coffee&logoColor=white)](https://www.buymeacoffee.com/TheFes)
[![Paypal](https://img.shields.io/badge/PayPal-00457C?&color=00457c&logo=paypal&logoColor=white)](https://www.paypal.com/paypalme/thefes)

# Home Assistant Voice Blueprints
![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

## Under construction

![Image](/images/Under_construction_graphic.gif?raw=true)

## About this repository

On this respository I will put my blueprints which are created to help issuing advanced (voice) commands using Assist. 
There are basically 5 ways to configure a voice pipeline in Home Assistant:
1. Local only
2. Using an LLM (like OpenAI, or Ollama) and with home control disabled for that LLM
3. As 2. but with local preference enabled.
4. Using an LLM with home control disabled.
5. As 4. but with local preference enabled.

On this repository different blueprints will be placed with the same goal. The will differ in which op the above mentioned configurations are supported. Based on that, also other aspects will differ.

## Blueprint options

### Option 1: Local

|Description|Pro/Con|
|---|---|
|Supported voice configurations|1, 2, 4|
|Fully local, no LLM required|✅|
|No home control for LLM required|✅|
|No strict voice commands|❌|
|Complete freedom in voice commands|❌|
|Variance in responses for each request|❌|
|Works without need for translations|❌|

### Option 2: LLM Enhanced automation

|Description|Pro/Con|
|---|---|
|Supported voice configurations|3, 5|
|Fully local, no LLM required|❌|
|No home control for LLM required|✅|
|No strict voice commands|✅|
|Complete freedom in voice commands|❌|
|Variance in responses for each request|✅|
|Works without need for translations|❌|

### Option 3: Full LLM script

|Description|Pro/Con|
|---|---|
|Supported voice configurations|3, 4, 5|
|Fully local, no LLM required|❌|
|No home control for LLM required|❌|
|No strict voice commands|✅|
|Complete freedom in voice commands|✅|
|Variance in responses for each request|✅|
|Works without need for translations|✅|

## The Blueprints

### Weather Forecasts

|Option|Import Button|Explanation|
|---|---|---|
|1: Local|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F1_voice_weather_forecast_local.yaml)|To be created|
|2: LLM Enhanced|To be created|To be created|
|3: Full LLM|[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fweather%2F3_voice_weather_forecast_full_llm.yaml)|To be created|

## ☕ Coffee

If you think I deserve a coffe, please feel free to buy me one (I might spend it on another beverage though).
In case you decide to do so, thanks a lot!

<a href="https://www.buymeacoffee.com/thefes" target="_blank">![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)</a>

Or you can do a small donation using PayPal.

[![paypal](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/paypalme/thefes)