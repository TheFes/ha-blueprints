![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Get ToDo items Entries - Full LLM

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Ftodo%2F1_voice_get_todo_entries_local.yaml)

Using this blueprint you can create an automation to script to list or are questions about todo items.

## Limitations of this script

* You need to have an LLM integration set up and use it in your voice pipeline
* The LLM integration needs access to your house (Assist), otherwise it can't access the script as a tool

## How to use the blueprint

1. Import the blueprint using the button above
2. Create an script using the blueprint
3. Select the weather entity to be used for the forecasts
4. (optionally) adjust the prompts which will are used to instruct the LLM how to use the script fields
5. Make sure to provide a clear description for the script (example below)
6. Save the script, and expose it to Assist

### Example for script description:

> Fetch todo items from my todo lists. Note that the shopping list is also considered a todo list. Ignore items marked as completed, unless the uses specifically requests them.

### Usage

You can request for the todo items in any way you can think of, using any language. Unless set otherwise in the LLM configuration, the response will be in the same language as the command.

#### Examples

```
What's on the shopping list?
Are there carrots on the shopping list?
Which todo items are planned for today? (only in case the todo items have due dates)
```