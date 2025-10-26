# Description

Translation for: [1_voice_weather_forecast_local](/weather/1_voice_weather_forecast_local.yaml)
Translation by: @felixschndr
Last update: 2025-23-10

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
      today:
      - heute
      tomorrow:
      - morgen
      day_after_tomorrow:
      - übermorgen
      monday:
      - montag
      tuesday:
      - dienstag
      wednesday:
      - mittwoch
      thursday:
      - donnerstag
      friday:
      - freitag
      saturday:
      - samstag
      sunday:
      - sonntag
      night:
      - nacht
      - nachts
      - heute nacht
      - morgen nacht
      morning:
      - morgen
      - morgens
      - früh
      - heute morgen
      - heute früh
      - morgen früh
      afternoon:
      - nachmittag
      - nachmittags
      - heute nachmittag
      - morgen nachmittag
      evening:
      - abend
      - abends
      - heute abend
      - morgen abend
      clear: und klar
      clear-night: und klar
      cloudy: und bewölkt
      fog: und neblig
      hail: mit Hagel
      lightning: mit Gewitter
      lightning-rainy: mit Gewitter und Regen
      partlycloudy: und teilweise bewölkt
      pouring: mit Starkregen
      rainy: und regnerisch
      snowy: mit Schnee
      snowy-rainy: mit Schnee und Regen
      sunny: und sonnig
      windy: und windig
      windy-variant: mit Wind und Wolken
      exceptional: mit außergewöhnlichem Wetter
      response_invalid_phrase: Ich kann keine Wettervorhersage für {{ phrase }} abrufen
      response_no_data: Keine Wettervorhersage für {{ phrase }} verfügbar
      response_forecast: >-
        Es {% if phrase %} wird{% else %} ist{% endif %} {{ temperature }} Grad {{
        condition_translated }}. 

        {%- if temperature != apparent_temperature -%}
          {% if phrase %}
            Es wird sich anfühlen wie {{ apparent_temperature }} Grad.
          {%- else -%}
            Es fühlt sich an wie {{ apparent_temperature }} Grad.
          {% endif %}
        {% endif %}

        {%- if precipitation_probability > rain_warning_threshold -%}
          Es besteht eine {{ precipitation_probability }}% Wahrscheinlichkeit für {{
          precipitation }} Millimeter Regen – nimm also besser einen Regenschirm
          mit, wenn du rausgehst.
        {% endif %}

        {%- if uv_index > 3 and condition in ['clear', 'partlycloudy'] -%}
          {%- if phrase -%}
            Die Sonne wird stark scheinen.
          {%- else -%}
            Die Sonne scheint stark.
          {%- endif -%}
        {% endif %}

        {%- if wind_speed >= wind_threshold -%}
          {%- if phrase -%}
            Der Wind wird {{ wind_phrase }} sein.
          {%- else -%}
            Der Wind is {{ wind_phrase }}.
          {%- endif -%}
        {% endif %}

        {%- if wind_speed >= wind_warning_threshold -%}
          Wenn du also nicht unbedingt raus musst, bleib besser drinnen.
        {%- elif wind_speed >= wind_joke_threshold -%}
          Pass auf, dass dir dein Regenschirm nicht davonfliegt!
        {% endif %}
      wind_phrases:
        windstill: 0
        schwach: 1
        mäßig: 12
        ziemlich kräftig: 29
        kräftig: 39
        stark: 50
        stürmisch: 62
        ein Sturm: 75
        ein schwerer Sturm: 89
        ein sehr schwerer Sturm: 103
        ein Orkan: 118
      trigger:
      - Wie (wird|ist) das Wetter [[am|diese|diesen|kommende|kommenden] {phrase}]
      - (Was|Wie) (ist|lautet) die [Wetter]vorhersage [[für] [diese|diesen|kommende|kommenden] {phrase}]
```
