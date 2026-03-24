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
6. Reload AUTOMATIONS in developer tools > YAML
7. (optionally) make further adjustments by clicking on the automation in Settings > Automations & Scenes

# Translations

```yaml
      trigger:
      - Wat is het {phrase} weer
      - Wat is het weer {phrase}
      - Wat voor weer (is|wordt) het [[deze|komende] {phrase}]
      - Wat is de [weers]voorspelling [[voor] [deze|komende] {phrase}]
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
      current:
      - nu
      - op dit moment
      - huidige
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
      windy-variant: met wind en wolken
      exceptional: met zeer uitzonderlijk weer
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
      wind_direction:
        N: noorden
        NE: noordoosten
        E: oosten
        SE: zuidoosten
        S: zuiden
        SW: zuidwesten
        W: westen
        NW: noordwesten
      response_invalid_phrase: Ik kan geen weersvoorspelling ophalen voor {{ phrase
        }}
      response_no_data: Geen weersvoorspelling beschikbaar voor {{ phrase }}
      response_forecast: Het {{ 'is' if current else 'wordt' }} {{ temperature }}
        graden {{ condition_translated }}. {% if temperature != apparent_temperature
        %}Het voelt als {{ apparent_temperature }} graden.{% endif %} {% if precipitation_probability
        > rain_warning_threshold %}Er is {{ precipitation_probability }}% kans op
        {{ precipitation }} milimeter regen, dus neem een paraplu mee als je naar
        buiten gaat.{% endif %} {% if uv_index > 3 and condition in ['clear', 'partlycloudy']
        %}De zon schijnt fel vandaag, dus doe zonnebrand op als je naar buiten gaat.{%
        endif %} {% if wind_speed >= wind_threshold %} De wind wordt {{ wind_phrase
        }} en komt uit het {{ wind_direction}}. {% endif %} {% if wind_speed >= wind_warning_threshold
        %}Dus als je niet per se naar buiten hoeft, blijf dan vooral lekker binnen.{%
        elif wind_speed >= wind_joke_threshold %}Dus zorg dat je {% if precipitation_probability
        > 10 %}paraplu {% else %}'t{% endif %}niet weg waait!{% endif %}
      response_current: "Het is nu {{ current_temperature }} graden en {{ current_condition_translated
        }}. De temperatuur zal vandaag tussen de {{ templow }} en {{ temperature }}
        graden zijn. {% if current_temperature != current_apparent_temperature %}Het
        voelt nu als {{ current_apparent_temperature }} graden.{% endif %} {% if precipitation_probability
        | float(-1) > rain_warning_threshold %}Er is  {{ precipitation_probability
        }}% kans op {{ precipitation }} mm regen, dus neem een paraplu mee naar buiten.{%
        endif %} {% if current_uv_index | float(-1) > 3 and current_condition in ['clear',
        'partlycloudy'] %}De zon schijnt fel vandaag, dus doe zonnebrand op als je
        naar buiten gaat.{% endif %} {%- if current_wind_speed is not none -%}\n  {%
        if current_wind_speed >= wind_threshold %} De wind is {{ current_wind_phrase
        }} en komt uit het {{ current_wind_direction }}.{% endif %}\n  {% if current_wind_speed
        >= wind_warning_threshold %}Dus als je niet per se naar buiten hoeft, kun
        je dat beter niet doen.{% elif current_wind_speed >= wind_joke_threshold %}Dus
        houd je paraplu goed vast!{% endif %}\n{%- endif %}"
```
