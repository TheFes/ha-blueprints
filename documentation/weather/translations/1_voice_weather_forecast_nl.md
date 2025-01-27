# Description

Translation for: [1_voice_weather_forecast_local](/weather/1_voice_weather_forecast_local.yaml)
Translation by: @TheFes
Last update: 2025-27-01

# How to use this translation

1. Import the blueprint
2. Create an automation using the blueprint
3. Open your automations.yaml using a file editor (for example using the File Editor or Visual Code Studio add-on)
4. Find the automation (it should be at the bottom if you just created it)
5. Paste the YAML code shown below right under the automation
5. Save the file
6. Relaod AUTOMATIONS in developer tools > YAML
7. (optionally) make further adjustments by clicking on the automation in Settings > Automations & Scenes

# Translations

```yaml
      today:
      - vandaag
      tomorrow:
      - morgen
      day_after_tomorrow:
      - overmorgen
      monday:
      - maandag
      tuesday:
      - dinsdag
      wednesday:
      - woensdag
      thursday:
      - donderdag
      friday:
      - vrijdag
      saturday:
      - zaterdag
      sunday:
      - zondag
      night:
      - vannacht
      - morgennacht
      - morgen nacht
      morning:
      - ochtend
      - morgenvroeg
      - morgen vroeg
      afternoon:
      - middag
      - vanmiddag
      - morgenmiddag
      evening:
      - morgenavond
      - vanavond
      - morgen avond
      clear: en helder
      clear-night: en helder
      cloudy: en bewolkt
      fog: en misting
      hail: met hagel
      lightning: met onweer
      lightning-rainy: met onweer en regen
      partlycloudy: en gedeeltelijk bewolkt
      pouring: met stortregen
      rainy: met regen
      snowy: met sneeuw
      snowy-rainy: met sneeuw en regen
      sunny: en zonnig
      windy: en winderig
      windy-variant: met wind en woken
      exceptional: met zeer uitzonderlijk weer
      response_invalid_phrase: Ik kan geen weersvoorspelling ophalen voor {{ phrase
        }}
      response_no_data: Geen weersvoorspelling beschikbaar voor {{ phrase }}
      response_forecast: Het wordt {{ temperature }} graden {{ condition_translated
        }}. {% if temperature != apparent_temperature %}Het voelt als {{ apparent_temperature
        }} graden.{% endif %} {% if precipitation_probability > rain_warning_threshold
        %}Er is {{ precipitation_probability }}% kans op {{ precipitation }} milimeter
        regen, dus neem een paraplu mee als je naar buiten gaat.{% endif %} {% if
        uv_index > 3 and condition in ['clear', 'partlycloudy'] %}De zon schijnt fel
        vandaag, dus doe zonnebrand op als je naar buiten gaat.{% endif %} {% if wind_speed
        >= wind_threshold %} De wind wordt {{ wind_phrase }}.{% endif %} {% if wind_speed
        >= wind_warning_threshold %}Dus als je niet per se naar buiten hoeft, blijf
        dan vooral lekker binnen.{% elif wind_speed >= wind_joke_threshold %}Dus zorg
        dat je {% if precipitation_probability > 10 %}paraplu {% else %}'t{% endif
        %}niet weg waait!{% endif %}
      wind_phrases:
        windstil: 0
        zwak: 1
        matig: 12
        vrij krachtig: 29
        krachtig: 39
        hard: 50
        stormachtig: 62
        een storm: 75
        een zware storm: 89
        een zeer zware storm: 103
        een orkaan: 118
      trigger:
      - Wat voor weer wordt het [[deze|komende] {phrase}]
      - Wat is de [weers]voorspelling [[voor] [deze|komende] {phrase}]
```