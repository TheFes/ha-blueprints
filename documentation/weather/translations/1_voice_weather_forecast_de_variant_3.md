# Description

Translation for: [1_voice_weather_forecast_local](/weather/1_voice_weather_forecast_local.yaml)
Translation by: @PocketMiner82
Last update: 2026-03-06

# How to use this translation

1. Import the blueprint
2. Create an automation using the blueprint
3. Edit the created automation in YAML mode
4. Paste the YAML code shown below right under the automation
5. Save the automation
6. (optionally) make further adjustments by clicking on the automation in Settings > Automations & Scenes

# Translations

```yaml
    today:
      - heute
    tomorrow:
      - morgen
    day_after_tomorrow:
      - übermorgen
      - über morgen
    monday:
      - montag
      - am montag
      - diese montag
      - diesen montag
      - kommende montag
      - kommenden montag
    tuesday:
      - dienstag
      - am dienstag
      - diese dienstag
      - diesen dienstag
      - kommende dienstag
      - kommenden dienstag
    wednesday:
      - mittwoch
      - am mittwoch
      - diese mittwoch
      - diesen mittwoch
      - kommende mittwoch
      - kommenden mittwoch
    thursday:
      - donnerstag
      - am donnerstag
      - diese donnerstag
      - diesen donnerstag
      - kommende donnerstag
      - kommenden donnerstag
    friday:
      - freitag
      - am freitag
      - diese freitag
      - diesen freitag
      - kommende freitag
      - kommenden freitag
    saturday:
      - samstag
      - am samstag
      - diese samstag
      - diesen samstag
      - kommende samstag
      - kommenden samstag
    sunday:
      - sonntag
      - am sonntag
      - diese sonntag
      - diesen sonntag
      - kommende sonntag
      - kommenden sonntag
    night:
      - nacht
      - diese nacht
      - diesen nacht
      - kommende nacht
      - kommenden nacht
      - heute nacht
      - morgen nacht
      - nachts
    morning:
      - am morgen
      - diese morgen
      - diesen morgen
      - kommende morgen
      - kommenden morgen
      - heute morgen
      - morgens
      - früh
      - heute früh
      - morgen früh
    afternoon:
      - nachmittag
      - am nachmittag
      - diese nachmittag
      - diesen nachmittag
      - kommende nachmittag
      - kommenden nachmittag
      - heute nachmittag
      - morgen nachmittag
      - nachmittags
    evening:
      - abend
      - am abend
      - diese abend
      - diesen abend
      - kommende abend
      - kommenden abend
      - heute abend
      - morgen abend
      - abends
    clear: >-
      {% if phrase %} wird{% else %} ist{% endif %} klar{% if phrase %} sein{% endif %}
    clear-night: >-
      {% if phrase %} wird{% else %} ist{% endif %} klar{% if phrase %} sein{% endif %}
    cloudy: >-
      {% if phrase %} wird{% else %} ist{% endif %} bewölkt{% if phrase %} sein{% endif %}
    fog: >-
      {% if phrase %} wird{% else %} ist{% endif %} neblig{% if phrase %} sein{% endif %}
    hail: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Hagel{% if phrase %} geben{% endif %}
    lightning: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Gewitter{% if phrase %} geben{% endif %}
    lightning-rainy: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Gewitter und Regen{% if phrase %} geben{% endif %}
    partlycloudy: >-
      {% if phrase %} wird{% else %} ist{% endif %} teilweise bewölkt{% if phrase %} sein{% endif %}
    pouring: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Starkregen{% if phrase %} geben{% endif %}
    rainy: >-
      {% if phrase %} wird{% else %} ist{% endif %} regnerisch{% if phrase %} sein{% endif %}
    snowy: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Schnee{% if phrase %} geben{% endif %}
    snowy-rainy: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Schnee und Regen{% if phrase %} geben{% endif %}
    sunny: >-
      {% if phrase %} wird{% else %} ist{% endif %} sonnig{% if phrase %} sein{% endif %}
    windy: >-
      {% if phrase %} wird{% else %} ist{% endif %} windig{% if phrase %} sein{% endif %}
    windy-variant: >-
      {% if phrase %} wird{% else %} gibt{% endif %} Wind und Wolken{% if phrase %} geben{% endif %}
    exceptional: >-
      {% if phrase %} wird{% else %} gibt{% endif %} außergewöhnliches Wetter{% if phrase %} geben{% endif %}
    response_invalid_phrase: >-
      Ich kann keine Wettervorhersage für {{ phrase }} abrufen, da der geforderte Zeitraum nicht unterstützt wird
    response_no_data: >-
      Es ist keine Wettervorhersage für {{ phrase }} verfügbar
    response_forecast: |-
      {% if phrase %}
        Es {{ condition_translated }}. Die Temperatur wird {{ phrase }} bei maximal {{ temperature }}{% if templow is defined %} und minimal {{ templow }}{% endif %} Grad liegen.
        Die Luftfeuchtigkeit wird etwa {{ humidity }} Prozent betragen.
      {%- else -%}
        Es {{ condition_translated }} mit einer Temperatur von {{ state_attr(weather_entity, 'temperature') }} Grad und einer Luftfeuchtigkeit von {{ state_attr(weather_entity, 'humidity') }} Prozent.
        Die Temperatur wird heute bei maximal {{ temperature }}{% if templow is defined %} und minimal {{ templow }}{% endif %} Grad liegen.
      {% endif %}

      {%- if temperature != apparent_temperature -%}
        {% if phrase %}
          Es wird sich anfühlen wie {{ apparent_temperature }} Grad.
        {%- else -%}
          Es fühlt sich an wie {{ apparent_temperature }} Grad.
        {% endif %}
      {% endif %}

      {%- if precipitation_probability > rain_warning_threshold -%}
        Es besteht eine {{ precipitation_probability }}% Wahrscheinlichkeit für {{ precipitation }} Liter Regen pro Quadratmeter. Nimm also besser einen Regenschirm mit, wenn du rausgehst.
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
          Der Wind ist {{ wind_phrase }}.
        {%- endif -%}
      {% endif %}

      {%- if wind_speed >= wind_warning_threshold -%}
        Wenn du also nicht unbedingt raus musst, bleib besser drinnen.
      {%- elif wind_speed >= wind_joke_threshold -%}
        Pass auf, dass dir dein Regenschirm nicht davonfliegt!
      {% endif %}
    wind_phrases:
      windstill: 0
      leise ziehend: 1
      leicht: 6
      schwach: 12
      mäßig: 20
      frisch: 29
      stark: 39
      steif: 50
      stürmisch: 62
      ein Sturm: 75
      ein schwerer Sturm: 89
      orkanartig: 103
      ein Orkan: 118
    trigger:
      - Wie (wird|ist) das Wetter [{phrase}]
      - (Was|Wie) (ist|lautet) die [Wetter]vorhersage [[für] {phrase}]
```
