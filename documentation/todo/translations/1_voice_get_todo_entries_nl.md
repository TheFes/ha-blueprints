# Description

Translation for: [1_voice_get_todo_entries_local](/todo/1_voice_get_todo_entries_local.yaml)
Translation by: @TheFes
Last update: 2025-30-01

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
      - "wat staat er op [de] (boodschappen[ ]lijst|lijst (met de naam|genaamd) 
        {list_name}|{list_name}[ ]lijst)"
      no_items_response: '{list_name} is leeg.'
      list_items_response: 'Er staan {item_count} dingen op: {list_items}'
      invalid_list_response: Ik kan geen lijst met de naam {list_name} vinden.
```