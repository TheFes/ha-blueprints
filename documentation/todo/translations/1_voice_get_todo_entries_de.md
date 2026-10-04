# Description

Translation for: [1_voice_get_todo_entries_local](/todo/1_voice_get_todo_entries_local.yaml)

Translation by: @sascha-hemi

Last update: 2026-10-04

# How to use this translation

1. Import the blueprint
2. Create an automation using the blueprint
3. Edit the created automation in YAML mode
4. Paste the YAML code shown below right under the automation (at the same indentation as `default_entity`)
5. Save the automation
6. (optionally) make further adjustments by clicking on the automation in Settings > Automations & Scenes

The sentence with `namens` / `mit dem namen` is listed first on purpose: otherwise "liste {list_name}" also matches it and takes "namens …" as part of the list name.

`einkaufsliste` and `was muss ich einkaufen` use the default todo list set in the blueprint.

Example sentences:

* Was steht auf der Einkaufsliste?
* Was muss ich einkaufen?
* Was steht auf der Liste Baumarkt?
* Was steht auf der Baumarktliste?
* Was steht auf der Liste namens Blueprints erstellen?
* Lies mir die Einkaufsliste vor

# Translations

```yaml
    trigger:
      - "was steht auf [der|meiner] liste (namens|mit dem namen) {list_name}"
      - "was steht auf [der|meiner] (einkaufsliste|liste {list_name}|{list_name}[ ]liste)"
      - "was (muss|soll) ich einkaufen"
      - "lies [mir] [die|meine] (einkaufsliste|liste {list_name}|{list_name}[ ]liste) vor"
    no_items_response: "{list_name} ist leer."
    list_items_response: "Einträge auf der Liste ({item_count}): {list_items}"
    invalid_list_response: "Ich kann keine Liste mit dem Namen {list_name} finden."
```
