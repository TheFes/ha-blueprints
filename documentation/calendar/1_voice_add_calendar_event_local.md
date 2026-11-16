<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Add Calendar Event - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F1_voice_add_calendar_event_local.yaml)

Using this blueprint you can create an automation to add events to your calendars without an LLM.

## Follow-up questions

* **On a voice satellite**: when the moment is not recognized is missing, Assist gives no response, and a second run of the automation (started with the `voice_calendar_event_follow_up` event) waits until the satellite is idle and asks for it with `assist_satellite.ask_question`. This repeats until the moment is recognized, or the maximum number of questions is reached. The confirmation is announced on the satellite
* **In text chat** (or with the maximum number of questions set to 0): the response says what is missing, and you need to say the complete request again

## How the sentence is interpreted

The default sentence is `(add|put|plan|schedule) {event} (to|on|in) [the|my] [{calendar}] (calendar|agenda) {due}`.

**Calendar**: the calendar is found in the complete sentence, so events with "to" or "in" in them ("go to the gym") work as well. After one of the `Words between the event and the calendar` ("to the", "in my") the blueprint looks for the name of one of your calendars, with or without a word for calendar after it ("work calendar", or "werkagenda" for the calendar "Werk"). The word for calendar on its own ("my calendar", "the agenda") and the `Default calendar names` refer to the default calendar. When the name is not recognized, the `Calendar not found` response is given.

**Moment**: everything after the calendar name. It can contain:

* a day (`today`, `tomorrow`, `day after tomorrow`) or weekday (the next occurrence, so on a Friday "friday" is next week)
* a start time (`17:30`, `17.30`, `5 pm` or `17`)
* an end time after one of the `Words before the end time` (`from 2 to 4 pm`, `at 9 until 11`). `pm` applies to both times, and an end time before the start time is moved to the afternoon ("from 10 to 2" ends at 14:00)
* a duration after one of the `Words before a duration` (`for 2 hours`, `for an hour`)
* a part of the day (`morning`, `afternoon`, `evening`, `tonight`). With a time ("7 in the evening") 12 hours are added in the afternoon and evening

## How the event is added

* Without a time an **all-day event** is added. An all-day event can last more than one day ("from Friday to Sunday")
* With a start time but without an end time or duration, the **default duration** (60 minutes) is used
* When only a time is mentioned, today is used, or tomorrow when that time has already passed
* Only calendars which support adding events can be used (for example Local Calendar, Google Calendar and CalDAV). Calendars of other integrations are skipped

## Limitations

* The sentences need to match the trigger sentences
* Events which last more than one day, dates like "December 3rd" and numbers as words ("five") are not supported
* The default sentence looks a lot like the sentence of the [Add ToDo item with due date](../todo/1_voice_add_todo_item_local.md) blueprints. When you use both, change the trigger sentences of one of them, so only one of them handles a sentence
* Follow-up questions only work on Assist satellites which support `ask_question` (like the Home Assistant Voice Preview Edition)

## Blueprint settings

Next to the calendar settings below, the trigger sentences, all words and phrases and the responses can be changed, so you can translate the blueprint. The responses can contain `{event}`, `{calendar}` and `{due}` (the moment, formatted with the `Date format`, `Word before the start time`, `Time format` and `Word before the end time`).

|Setting|Description|
|---|---|
|Calendars|The calendars events can be added to. Leave empty to allow all calendars which support adding events|
|Default calendar|The calendar used when no calendar is mentioned|
|Default duration|The duration in minutes when only a start time is mentioned, default 60|
|Default calendar names|Other names which refer to the default calendar, for example `family`|
|Words for calendar|Default `calendar` and `agenda`|
|Words between the event and the calendar|Longer variants first|
|Maximum number of follow-up questions|Default 2, set to 0 to never ask|

## Translation

### Dutch

```yaml
calendar_words:
  - agenda
  - kalender
calendar_separators:
  - in de
  - in mijn
  - op de
  - op mijn
  - in
  - op
trigger:
  - "(zet|plan) {event} (in|op) [de|mijn] [{calendar}] (agenda|kalender) {due}"
day_phrases:
  vandaag: 0
  vanavond: 0
  vannacht: 0
  morgen: 1
  morgenochtend: 1
  morgenmiddag: 1
  morgenavond: 1
  overmorgen: 2
weekday_phrases:
  maandag: 0
  dinsdag: 1
  woensdag: 2
  donderdag: 3
  vrijdag: 4
  zaterdag: 5
  zondag: 6
day_part_phrases:
  ochtend: "09:00"
  "'s ochtends": "09:00"
  morgenochtend: "09:00"
  middag: "14:00"
  "'s middags": "14:00"
  morgenmiddag: "14:00"
  avond: "19:00"
  "'s avonds": "19:00"
  morgenavond: "19:00"
  vanavond: "20:00"
pm_phrases: {}
until_words:
  - tot
duration_words:
  - voor
duration_units:
  minuut: 1
  minuten: 1
  kwartier: 15
  uur: 60
  uren: 60
one_words:
  - een
  - één
date_format: "%A %-d %B"
time_format: "%H:%M"
from_word: van
to_word: tot
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
response_added: "{event} staat in {calendar} op {due}"
response_invalid_calendar: Ik kan geen agenda met de naam {calendar} vinden
question_due: Wanneer is {event}?
response_invalid_due: Ik weet niet wanneer {due} is, dus ik heb {event} niet toegevoegd
response_no_answer: Ik heb geen antwoord gehoord, dus ik heb {event} niet toegevoegd
response_error: Het is niet gelukt om {event} aan {calendar} toe te voegen
```
