<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Set Reminders - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Freminder%2F1_voice_reminder_local.yaml)

Using this blueprint you can create an automation to set reminders without an LLM.

## Follow-up questions

* **On a voice satellite**: when the recipient or the moment is missing, or not recognized, Assist gives no response, and a second run of the automation (started with the `voice_reminder_follow_up` event) waits until the satellite is idle and asks for it with `assist_satellite.ask_question` ("At what time should I remind you?"). This repeats until everything is known, or the maximum number of questions is reached. The confirmation is announced on the satellite
* **In text chat** (or with the maximum number of questions set to 0): the response contains the question, and you need to say the complete request again including the answer

## Who is "me"?

"Me" refers to the recipient whose person is linked to the user talking to Assist. When you use a voice satellite, there is no user, so "me" refers to the recipient which has that satellite set (when only one recipient has it). Otherwise Assist asks who should be reminded.

## Requirements

* Home Assistant 2025.7 or newer
* A todo list which supports due dates with a time and descriptions. The [Local To-do](https://www.home-assistant.io/integrations/local_todo/) integration works fine, preferably create a separate list for the reminders
* An automation based on the [Send Reminder (version 2)](0_voice_send_reminder_v2.md) blueprint to send the reminders

## How the sentence is interpreted

The default sentences are:

* `remind {who} to {reminder}`
* `(set|create|make|add) a reminder for {who} to {reminder}`

**Recipients**: `{who}` is split on the `Words between names` (`and`), and every part is matched with the names and other names of the recipients (not case sensitive). `me` is the person speaking, `everyone` (or `us`) are all recipients. When a name is not recognized, Assist asks who should be reminded.

**Moment**: the moment is taken from the end of `{who}` ("remind me *tomorrow at 9* to ..."), the end of `{reminder}` ("... to take out the trash *tomorrow at 7*") and the start of `{reminder}` ("... eraan om *morgen om 7* de vuilnis buiten te zetten"). It is the longest series of words which are all part of the moment phrases (days, weekdays, parts of the day, units, `Other words in the moment`) or times, and it needs to contain at least a day, part of the day, unit or number. So "to meet at the station tomorrow at 7 pm" becomes "meet at the station" at tomorrow 19:00.

The moment can contain:

* a day (`today`, `tomorrow`, `day after tomorrow`) or weekday (the next occurrence, so on a Friday "friday" is next week)
* a time (`17:30`, `17.30`, `5 pm` or `17`)
* a part of the day (`morning`, `afternoon`, `evening`, `tonight`). With a time ("7 in the evening") 12 hours are added in the afternoon and evening
* a relative moment (`in 10 minutes`, `in an hour`)

When only a time is mentioned, today is used, or tomorrow when that time has already passed. When only a day is mentioned, Assist asks for the time.

## Limitations

* The sentences need to match the trigger sentences
* A moment in the middle of the reminder ("remind me that grandma comes on Friday evening") is not recognized
* Dates like "December 3rd" and numbers as words ("five") are not supported
* Follow-up questions only work on Assist satellites which support `ask_question` (like the Home Assistant Voice Preview Edition)

## Recipients

The recipients are set with an object selector, with these fields for each recipient:

|Field|Description|
|---|---|
|Name|The name of the recipient, for example `Martijn`. Can also be a group, like `The kids`|
|Other names or description|Comma separated, for example `dad, father`. Used to recognize the recipient|
|Person|Optional. Used to know who "me" is (the user linked to the person), and the Assist satellites are only used when this person is home|
|Notify entities|The notify entities which receive the reminder, for example the notify entity of the Companion app on your phone|
|Assist satellites|Optional. The reminder is announced on these satellites when the person is home (or always when no person is set)|

In YAML it looks like this:

```yaml
- name: Martijn
  aliases: dad
  person: person.martijn
  notify:
    - notify.pixel_10_pro
  satellites:
    - assist_satellite.office
- name: Marleen
  person: person.marleen
  notify:
    - notify.iphone_van_marleen
```

## How the reminders are stored

The reminder is added to the todo list with the text as summary and the moment as due date and time. The recipients are stored in the description, one line per recipient with the entities to use, for example:

```
Martijn: notify.pixel_10_pro, assist_satellite.office, person.martijn
```

The [Send Reminder (version 2)](0_voice_send_reminder_v2.md) automation reads this description, so it doesn't need an LLM or its own list of recipients. Changing the recipients in the blueprint doesn't change reminders which were already set.

## Blueprint settings

Next to the todo list and recipients, the trigger sentences, all words and phrases and the responses can be changed, so you can translate the blueprint. The responses can contain:

|Placeholder|Description|
|---|---|
|`{text}`|The reminder|
|`{recipients}`|The recipients, with the person speaking as `you`|
|`{due}`|The moment, formatted with the `Date format`, `Word between date and time` and `Time format`|
|`{names}`|Names which were not recognized|
|`{question}`|The question for the missing information (only in `Information missing`)|

## Translation

### Dutch

```yaml
trigger:
  - "herinner {who} (eraan|er aan) [om|dat] {reminder}"
  - "herinner {who} aan {reminder}"
  - "(zet|maak) een herinnering voor {who} (om|dat) {reminder}"
me_words:
  - me
  - mij
  - mijzelf
everyone_words:
  - iedereen
  - ons
  - allemaal
name_separators:
  - en
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
relative_words:
  - over
  - binnen
relative_units:
  minuut: 1
  minuten: 1
  kwartier: 15
  uur: 60
one_words:
  - een
  - één
due_words:
  - om
  - op
  - deze
  - volgende
  - komende
  - over
  - binnen
  - voor
  - rond
  - uur
date_format: "%A %-d %B"
time_format: "%H:%M"
at_word: om
and_word: en
you_word: je
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
response_set: "Ik herinner {recipients} op {due} aan: {text}"
question_recipients: Wie moet ik herinneren?
question_unknown: Ik weet niet wie {names} is. Wie moet ik herinneren?
question_due: Wanneer moet ik {recipients} herinneren?
question_time: Hoe laat moet ik {recipients} herinneren?
question_past: Dat moment is al voorbij. Wanneer moet ik {recipients} herinneren?
response_missing: "{question} Zeg je hele verzoek nog een keer, inclusief dit."
response_no_answer: Ik heb geen antwoord gehoord, dus ik heb de herinnering niet ingesteld
response_error: Sorry, het is niet gelukt om de herinnering in te stellen
```
