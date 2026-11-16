<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_dark.png?raw=true">
  <source media="(prefers-color-scheme: light)" srcset="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
  <img alt="thefes.casa blueprints logo" src="https://github.com/TheFes/ha-blueprints/blob/main/images/TheFesCasa_logo_bp_light.png?raw=true">
</picture>

# Set Reminders - Full LLM v2 (AI Task)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Freminder%2F3_voice_reminder_full_llm_v2.yaml)

Using this blueprint you can create a script which can be used as a tool by an LLM conversation agent to set reminders. It replaces the [first version](3_voice_reminder_full_llm.md), which is still available for existing users.

The LLM only passes the request in a single `query` field. An [AI Task](https://www.home-assistant.io/integrations/ai_task/) call (`ai_task.generate_data`) with structured output determines the text, the recipients (a select, so only configured recipients can be returned), the date and the time.

## Follow-up questions

The AI Task is told never to guess. When the text, the recipients, the date or the time is missing (or the moment is in the past), nothing is stored, and the script returns what is missing:

```json
{
  "success": false,
  "missing": ["time"],
  "instruction": "The reminder was NOT set. Ask the user for the missing information, then call this tool again with the complete request including the answer.",
  "understood": {"text": "take out the trash", "recipients": ["Martijn"], "date": "2026-11-17", "time": ""}
}
```

The LLM then asks you for it, and calls the script again with the complete request. So the LLM still asks follow-up questions until it has everything it needs, like in the first version.

When the reminder is set the script returns `success`, the text, the recipients and the moment, so the LLM can confirm it.

## Who is "me"?

When the user who is talking to the LLM is linked to a person, and that person is set for one of the recipients, "me" refers to that recipient. Otherwise (for example on a voice satellite) the script reports the recipients as missing when you say "me", so the LLM asks who should be reminded.

## Requirements

* Home Assistant 2025.7 or newer
* An AI Task entity. You can select it in the blueprint, or set a preferred AI Task entity in Home Assistant
* A todo list which supports due dates with a time and descriptions. The [Local To-do](https://www.home-assistant.io/integrations/local_todo/) integration works fine, preferably create a separate list for the reminders
* An automation based on the [Send Reminder (version 2)](0_voice_send_reminder_v2.md) blueprint to send the reminders

## Blueprint setup

* Create the script using the blueprint, and set the todo list and the recipients
* Give the script a clear description, the LLM uses it to decide when to use the script. For example:

  `Sets a reminder for one or more people at a date and time. Use this tool whenever the user asks to be reminded of something, or to remind someone else. Pass the complete request of the user in the query field, in the words of the user. When the tool reports missing information, ask the user for it and call the tool again with the complete request including the answer.`

* **Expose the script to Assist**

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

|Setting|Description|
|---|---|
|ToDo list|The list to store the reminders in|
|Recipients|See above|
|AI Task entity|Leave empty to use the preferred AI Task entity|
|Instructions for the AI|How the AI should interpret the request. `{{ query }}` is the request, `{{ speaker }}` the recipient who is speaking (if known), `{{ recipient_desc }}` the recipients with their other names|
|Query field description|Tells the LLM what to pass to the script|
