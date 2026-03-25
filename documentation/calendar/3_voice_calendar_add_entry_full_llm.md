![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Calendar entries - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Fcalendar%2F3_voice_calendar_add_entry_full_llm.yaml)

Using this blueprint you can create a script to create calendar entries. It allows you to create an event on a calendar by giving
a title and date with the option to include start and end times.

## Limitations of this script

* You need to have an LLM integration set up and use it in your voice pipeline
* The LLM integration needs access to your house (Assist), otherwise it can't access the script as a tool
* It sometimes add times to all day events when it shouldn't

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an script using the blueprint
3. Select one calendar entity
4. (optionally) adjust the prompts which will are used to instruct the LLM how to use the script fields
5. Make sure to provide a clear description for the script (example below)
6. Save the script, and expose it to Assist

### Example for script description:

>Create a calendar event in my calendar. In case the data for the weekend
    is requested, this means Saturday and Sunday. Only fill start_time and end_time if
    the user explicity gave a time; otherwise leave it blank. All day events
    which don't have times specified do not need a start_time or end_time!

## Usage

You can create a calendar entry using any language.
Unless set otherwise in the LLM configuration, the response will be in the same
language as the command.

You can specify a date and time or just a date to create the event. Not providing a
time will create an all day event in most cases. Sometimes the LLM will pick times
even if not provided. The script description is important to limit this interaction.


## Examples

```
Add go to the grocery store to my calendar tomorrow.
Create an event for go to the doctor at 9am on Tuesday.
Add jump rope to my calendar on Tuesday from noon to 3pm.
```


