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
    current:
      - gerade
      - aktuell
      - in diesem Moment
      - jetzt
      - in dieser Stunde
      - grad
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
      {% if not current %} wird{% else %} ist{% endif %} klar{% if not current %} sein{% endif %}
    clear-night: >-
      {% if not current %} wird{% else %} ist{% endif %} klar{% if not current %} sein{% endif %}
    cloudy: >-
      {% if not current %} wird{% else %} ist{% endif %} bewölkt{% if not current %} sein{% endif %}
    fog: >-
      {% if not current %} wird{% else %} ist{% endif %} neblig{% if not current %} sein{% endif %}
    hail: >-
      {% if not current %} wird{% else %} gibt{% endif %} Hagel{% if not current %} geben{% endif %}
    lightning: >-
      {% if not current %} wird{% else %} gibt{% endif %} Gewitter{% if not current %} geben{% endif %}
    lightning-rainy: >-
      {% if not current %} wird{% else %} gibt{% endif %} Gewitter und Regen{% if not current %} geben{% endif %}
    partlycloudy: >-
      {% if not current %} wird{% else %} ist{% endif %} teilweise bewölkt{% if not current %} sein{% endif %}
    pouring: >-
      {% if not current %} wird{% else %} gibt{% endif %} Starkregen{% if not current %} geben{% endif %}
    rainy: >-
      {% if not current %} wird{% else %} ist{% endif %} regnerisch{% if not current %} sein{% endif %}
    snowy: >-
      {% if not current %} wird{% else %} gibt{% endif %} Schnee{% if not current %} geben{% endif %}
    snowy-rainy: >-
      {% if not current %} wird{% else %} gibt{% endif %} Schnee und Regen{% if not current %} geben{% endif %}
    sunny: >-
      {% if not current %} wird{% else %} ist{% endif %} sonnig{% if not current %} sein{% endif %}
    windy: >-
      {% if not current %} wird{% else %} ist{% endif %} windig{% if not current %} sein{% endif %}
    windy-variant: >-
      {% if not current %} wird{% else %} gibt{% endif %} Wind und Wolken{% if not current %} geben{% endif %}
    exceptional: >-
      {% if not current %} wird{% else %} gibt{% endif %} außergewöhnliches Wetter{% if not current %} geben{% endif %}
    wind_direction:
      N: nördlicher
      NE: nordöstlicher
      E: östlicher
      SE: südöstlicher
      S: südlicher
      SW: südwestlicher
      W: westlicher
      NW: nordwestlicher
    response_invalid_phrase: >-
      Ich kann keine Wettervorhersage für {{ phrase }} abrufen, da der geforderte Zeitraum nicht unterstützt wird
    response_no_data: >-
      Es ist keine Wettervorhersage {{ phrase }} verfügbar
    response_current: >-
      Es {{ current_condition_translated }} mit einer Temperatur von {{ current_temperature }} Grad und einer Luftfeuchtigkeit von {{ current_humidity }} Prozent.
      Die Temperatur wird heute bei minimal {{ templow }} und maximal {{ temperature }} Grad liegen.

      {%- if current_temperature != current_apparent_temperature %} Es fühlt sich aber an wie {{ current_apparent_temperature }} Grad.
      {%- endif %}

      {%- if precipitation > 0 %} Heute besteht eine {{ precipitation_probability }} prozentige Wahrscheinlichkeit für {{ precipitation }} Liter Regen pro Quadratmeter.
        {%- if precipitation_probability > rain_warning_threshold %} Nimm also besser einen Regenschirm mit, wenn du rausgehst.
        {%- endif %}
      {%- endif %}

      {%- if current_uv_index > 3 and current_condition in ['clear', 'partlycloudy'] %} Die Sonne scheint stark.
      {%- endif %}

      {%- if current_wind_speed >= wind_threshold %} Der Wind weht {{ current_wind_phrase }} aus {{ current_wind_direction }} Richtung.
      {%- endif %}

      {%- if current_wind_speed >= wind_warning_threshold %} Wenn du also nicht unbedingt raus musst, bleib besser drinnen.
      {%- elif current_wind_speed >= wind_joke_threshold %} Pass auf, dass dir dein Regenschirm nicht davonfliegt!
      {%- endif %}
    response_forecast: >-
      Es {{ condition_translated }}. Die Temperatur wird {{ phrase }} bei{% if templow is defined %} minimal {{ templow }} und{% endif %} maximal {{ temperature }} Grad liegen.
      Die Luftfeuchtigkeit wird etwa {{ humidity }} Prozent betragen.

      {%- if temperature != apparent_temperature %} Es wird sich aber anfühlen wie {{ apparent_temperature }} Grad.
      {%- endif %}

      {%- if precipitation > 0 %} Es besteht eine {{ precipitation_probability }} prozentige Wahrscheinlichkeit für {{ precipitation }} Liter Regen pro Quadratmeter.
        {%- if precipitation_probability > rain_warning_threshold %} Nimm also besser einen Regenschirm mit, wenn du rausgehst.
        {%- endif %}
      {%- endif %}

      {%- if current_uv_index > 3 and current_condition in ['clear', 'partlycloudy'] %} Die Sonne scheint stark.
      {%- endif %}

      {%- if uv_index > 3 and condition in ['clear', 'partlycloudy'] %} Die Sonne wird stark scheinen.
      {%- endif %}

      {%- if wind_speed >= wind_threshold %} Der Wind wird {{ wind_phrase }} aus {{ wind_direction }} Richtung wehen.
      {%- endif %}

      {%- if wind_speed >= wind_warning_threshold %} Wenn du also nicht unbedingt raus musst, bleib besser drinnen.
      {%- elif wind_speed >= wind_joke_threshold %} Pass auf, dass dir dein Regenschirm nicht davonfliegt!
      {%- endif %}
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
      sturmartig: 75
      sehr sturmartig: 89
      orkanartig: 103
      sehr orkanartig: 118
    trigger:
      - >-
        [Wie] (wird|ist) [das] Wetter [[für] {phrase}]
      - >-
        [Was|Wie] (ist|lautet) [die] [Wetter]vorhersage [[für] {phrase}]
```
