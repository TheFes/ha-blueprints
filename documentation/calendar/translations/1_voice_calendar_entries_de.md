# Description

Translation for: [1_voice_calendar_entries_local](/calendar/1_voice_calendar_entries_local.yaml)
Translation by: @nilsreiter and @felixschndr
Last update: 2025-23-10

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
    trigger:
      - was (steht|ist|habe ich) [(für|am)] {phrase} (im kalender|auf der agenda|vor)
      - was passiert [am] {phrase}
      - welche Termine habe ich [am ]{phrase}
    today:
      - heute
    tomorrow:
      - morgen
    event_singular: Termin
    event_plural: Termine
    no_events: In diesem Zeitraum gibt es keine Termine.
    event_count: "Am {date} gibt es {event_count} {events}:"
    day_event: Ganztägig steht {event_list} im Kalender.
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
    morning:
      - vormittag
      - vormittags
      - morgen
      - morgens
    afternoon:
      - mittag
      - mittags
      - nachmittag
      - nachmittags
    evening:
      - abend
      - abends
    part_day_event: Von {start} bis {end} steht {event_summary} im Kalender.
    combine_word: und
    response_translations:
      Monday: Montag
      Tuesday: Dienstag
      Wednesday: Mittwoch
      Thursday: Donnerstag
      Friday: Freitag
      Saturday: Samstag
      Sunday: Sonntag
      January: Januar
      February: Februar
      March: März
      April: April
      May: Mai
      June: Juni
      July: Juli
      August: August
      September: September
      October: Oktober
      November: November
      December: Dezember
    date_format: "%A"
```
