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
      - heute nacht
      - morgen nacht
      morning:
      - morgen
      - früh
      - heute morgen
      - heute früh
      - morgen früh
      afternoon:
      - nachmittag
      - heute nachmittag
      - morgen nachmittag
      evening:
      - abend
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
      rainy: mit Regen
      snowy: mit Schnee
      snowy-rainy: mit Schnee und Regen
      sunny: und sonnig
      windy: und windig
      windy-variant: mit Wind und Wolken
      exceptional: mit außergewöhnlichem Wetter
      response_invalid_phrase: Ich kann keine Wettervorhersage für {{ phrase }} abrufen
      response_no_data: Keine Wettervorhersage verfügbar für {{ phrase }}
      response_forecast: Es wird {{ temperature }} Grad {{ condition_translated }}. {% if temperature != apparent_temperature %}Es fühlt sich an wie {{ apparent_temperature }} Grad.{% endif %} {% if precipitation_probability > rain_warning_threshold %}Es besteht ein {{ precipitation_probability }}-prozentiges Risiko für {{ precipitation }} Millimeter regen, also nimm einen Regenschirm mit, wenn du rausgehst.{% endif %} {% if uv_index > 3 and condition in ['clear', 'partlycloudy'] %}Die Sonne scheint stark heute, also trage Sonnenschutz auf, wenn du nach draußen gehst.{% endif %} {% if wind_speed >= wind_threshold %} Der Wind wird {{ wind_phrase }}.{% endif %} {% if wind_speed >= wind_warning_threshold %}Also, wenn du nicht unbedingt raus musst, bleib lieber drinnen.{% elif wind_speed >= wind_joke_threshold %}Also pass auf, dass dir {% if precipitation_probability > 10 %}dein Regenschirm {% else %}'s{% endif %} nicht wegfliegt!{% endif %}
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