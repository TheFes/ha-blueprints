# Description

Translation for: [1_voice_calendar_entries_local](/calendar/1_voice_calendar_entries_local.yaml)
Translation by: @TheFes
Last update: 2025-29-01

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
      response_translations:
        Monday: maandag
        Tuesday: dinsdag
        Wednesday: woensdag
        Thursday: donderdag
        Friday: vrijdag
        Saturday: zaterdag
        Sunday: zondag
        January: januari
        February: februari
        March: maart
        April: april
        May: mei
        June: juni
        July: juli
        August: augustus
        September: september
        October: oktober
        November: november
        December: december
      event_singular: afspraak
      event_plural: afspraken
      no_events: Er zijn geen afspraken voor deze periode
      event_count: Op {date} heb je {event_count} {events}.
      day_event: Voor de hele dag staat er {event_list}.
      part_day_event: Van {start} tot {end} heb je {event_summary}.
      trigger:
      - wat staat er [voor|op] {phrase} op de (agenda|kalender|planning)
      - staat er [voor|op] {phrase} iets op de (agenda|kalender|planning)
```