![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Add ToDo item with due date - Local

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F1_voice_add_todo_item_local.yaml)

Home Assistant has a built-in intent to add an item to a todo list, but it doesn't support a due date or time. Using this blueprint you can create an automation to add an item with a due date and time. The automation runs totally local, no LLM is required.

## Requirements

* Home Assistant 2024.10 or newer

## Limitations of this automation

* The sentences used to trigger the automation are rather strict
* The list needs to be one of your todo lists (or one of the `Default list names`), otherwise the `List not found` response is given
* The due moment can contain a day (today, tomorrow, the day after tomorrow or a weekday), a time and a part of the day. Dates like "December 3rd", "next week" or "in two hours" are not supported, and numbers need to be digits (`5`, not `five`)
* One item per sentence
* In case you want to use it in another language as English, you need to translate the trigger, the phrases and the responses. A Dutch translation is available below

## How the default sentence works

The default sentences are:

* `(add|put) {item} (on|to) [the|my] {list} (for|on|at|due|before|by) {due}`
* `(add|put) {item} (on|to) [the|my] {list} (today|tonight|tomorrow|this|next) [{due}]`

The word after `{list}` is required, so a sentence without a due moment ("add milk to the shopping list") and other sentences like "add 5 minutes to the timer" don't match, and are still handled by the built-in intents.

Sentence matching can't reliably split the wildcards itself (in "add go to the gym to my chores for friday" the item would be "go"), so the blueprint determines the item, the list and the due moment from the complete sentence:

1. It looks for the `Words between the item and the list` ("to the", "on my", ...) and checks if the text after them starts with the name of one of your todo lists or one of the `Default list names`. The longest matching name wins.
2. A list name is also recognized with one of the `Words for list` after it ("chores list", or "boodschappenlijst" for the list "Boodschappen"), and without it when the list name itself ends with it (the list "Homeassistant list" is found with both "homeassistant list" and "homeassistant").
3. Everything after the list name is the due moment.

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an automation using the blueprint
3. Select the default list
4. (optionally) change or translate the trigger sentences, phrases and responses

## Blueprint settings

### List settings

|Setting|Description|
|---|---|
|Default todo list|Used when the spoken list name is one of the `Default list names`|
|Default list names|For example `shopping` or `todo`. Other names are matched with the names of your todo lists (not case sensitive)|
|Words for list|Words for "list", see [How the default sentence works](#how-the-default-sentence-works)|
|Words between the item and the list|The words between `{item}` and `{list}` in your sentences, longer variants first (`to the` before `to`)|

### Settings for the due moment

|Setting|Description|
|---|---|
|Day phrases|Phrases for a day with the number of days from today, for example `tomorrow: 1`|
|Weekday phrases|Phrases for weekdays with the number of the weekday (0 is Monday). The next occurrence is used, so on a Friday "friday" is next week's Friday|
|Parts of the day|Phrases for parts of the day with the time used for them, for example `evening: "19:00"`. When a time is mentioned as well ("at 7 in the evening") and the part of the day is after 12:00, 12 hours are added to times before 12:00|
|Words for PM and AM|Words which mean pm or am|

How the due moment is determined:

* Times are recognized as `17:30`, `17.30`, `5 pm` or just `17` (also inside a phrase like "om 17 uur")
* When only a day is mentioned, only a due date is set
* When only a time or part of the day is mentioned, today is used, or tomorrow when that time has already passed
* When nothing is recognized, the item is **not** added and the `Due moment not recognized` response is given

### Settings for the responses

`{item}`, `{list_name}` and `{due}` are replaced in the responses. The date and time in `{due}` are formatted with the `Date format` and `Time format` settings, and the English weekday and month names can be translated with `Translations for weekdays and months`.

## Lists without due dates

Not every todo list supports due dates. For example the Shopping list integration doesn't support them at all, and some integrations only support a date without a time. The blueprint checks what the list supports: it sets a date and time when possible, otherwise only the date, and when the list doesn't support due dates the item is added without one, and the `List doesn't support due dates` response is given.

## Translation

### Dutch

```yaml
default_list_names:
  - boodschappen
  - taken
  - to do
  - todo
trigger:
  - "(zet|doe) {item} op [de|mijn] {list} (voor|op|om|tegen|deze|volgende|komende) {due}"
  - "(zet|doe) {item} op [de|mijn] {list} (vandaag|vanavond|vannacht|morgen|overmorgen|maandag|dinsdag|woensdag|donderdag|vrijdag|zaterdag|zondag) [{due}]"
  - "voeg {item} toe aan [de|mijn] {list} (voor|op|om|tegen|deze|volgende|komende) {due}"
  - "voeg {item} toe aan [de|mijn] {list} (vandaag|vanavond|vannacht|morgen|overmorgen|maandag|dinsdag|woensdag|donderdag|vrijdag|zaterdag|zondag) [{due}]"
list_words:
  - lijst
list_separators:
  - toe aan de
  - toe aan mijn
  - toe aan
  - op de
  - op mijn
  - op
day_phrases:
  vandaag: 0
  vanavond: 0
  vannacht: 0
  morgen: 1
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
  morgenochtend: "09:00"
  middag: "14:00"
  avond: "19:00"
  vanavond: "20:00"
pm_phrases:
  "'s middags": pm
  "'s avonds": pm
  "'s ochtends": am
at_word: om
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
response_added: "{item} staat op {list_name}, voor {due}"
response_no_due_support: "{item} staat op {list_name}, maar deze lijst ondersteunt geen datum"
response_invalid_list: "Ik kan geen lijst met de naam {list_name} vinden"
response_invalid_due: "Ik weet niet wanneer {due} is, dus ik heb {item} niet toegevoegd"
response_error: "Het is niet gelukt om {item} op {list_name} te zetten"
```

## Examples

These are real responses from testing:

- *Add call the plumber to the testerdetest list tomorrow at 10*: Added call the plumber to Testerdetest, due Tuesday 17 November at 10:00
- *Put buy a present on my todo list for friday*: Added buy a present to Testerdetest, due Friday 20 November
- *Add ginger to the boodschappen list tonight*: Added ginger to Boodschappen, but this list doesn't support due dates
- *Add something to the testerdetest list next year*: I don't know when next year is, so I didn't add something
- *Zet cadeau kopen op mijn todo lijst voor vrijdag om 3 uur 's middags*: cadeau kopen staat op Testerdetest, voor vrijdag 20 november om 15:00
