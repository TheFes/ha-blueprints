![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Calendar entries - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F3_voice_calendar_entries_full_llm.yaml)

Using this blueprint you can create an script to request for calendar entries. It allows you to ask any calendar related question, so not only directly for a day of period, but also if when the next doctor's meeting will be.

## Limitations of this script

* You need to have an LLM integration set up and use it in your voice pipeline
* The LLM integration needs access to your house (Assist), otherwise it can't access the script as a tool

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an script using the blueprint
3. Select one or more calendar entries
4. (optionally) adjust the prompts which will are used to instruct the LLM how to use the script fields
5. Make sure to provide a clear description for the script (example below)
6. Save the script, and expose it to Assist

### Example for script description:

>Fetch calendar events from my calendar. In case the data for the weekend is requested, this means Saturday and Sunday

## Usage

You can request for the calendar entries in any way you can think of, using any language. Unless set otherwise in the LLM
configuration, the response will be in the same language as the command.

You can refer to a single day, multiple days (e.g. next weekend) a part of the day (e.g. next morning) or one or more hours
(e.g. from 5 to 9).

You can ask for specific events like doctor's appointments or visits to the grandparents.


## Examples

```
What's on the calendar tomorrow?
Do I have something on the calendar this evening?
Am I busy tonight at 9?
Do I have a doctor's appointment next week?
```


