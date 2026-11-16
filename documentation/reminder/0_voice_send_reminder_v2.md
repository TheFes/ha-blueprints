![Image](https://github.com/TheFes/ha-blueprints/blob/main/images/header.png?raw=true)

# Send Reminder - Automation (version 2)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FTheFes%2Fha-blueprints%2Fblob%2Fmain%2Freminder%2F0_voice_send_reminder_v2.yaml)

This automation sends the reminders created by the version 2 reminder blueprints:

* [1: Local](1_voice_reminder_local.md)
* [2: LLM Enhanced](2_voice_reminder_llm_enhanced.md)
* [3: Full LLM v2 (AI Task)](3_voice_reminder_full_llm_v2.md)

Those blueprints store the recipients, with their notify entities and Assist satellites, in the description of the todo item. This automation reads that description, so it doesn't need an LLM or a list of recipients.

> This automation is not compatible with reminders created by the original full LLM script (version 1). Use the original [Send Reminder](../../reminder/0_voice_send_reminder.yaml) automation for those.

## Requirements

* Home Assistant 2025.7 or newer
* Notify entities for the notifications. The Companion app provides these for your phone

## How it works

Every minute (by default) the open items on the todo list are checked. For every reminder which is due (or due within 30 seconds):

1. A notification is sent to the notify entities of all recipients, using `notify.send_message`. When several recipients share a notify entity, it is only sent once
2. The reminder is announced on the Assist satellites of the recipients, but only for recipients whose person is home (or who don't have a person set). Recipients sharing a satellite get one announcement with all names
3. When at least one notification or announcement was done, the reminder is removed from the list, or marked as completed

When nothing could be sent, for example because the recipient only has satellites and is not home, the reminder stays on the list. It is tried again every minute, so it is announced as soon as the person gets home.

Reminders without recipients in the description (for example added manually in the todo list) are sent to the fallback notify entities, or as a persistent notification when no fallback is set.

## Limitations

* `notify.send_message` only supports a title and a message. Companion app specific options like a notification channel, priority or actions are not possible with notify entities

## Blueprint settings

|Setting|Description|
|---|---|
|ToDo list|The list which is used to store the reminders, the same list as in the blueprints to set reminders|
|After notification|`Delete item` or `Mark as completed`|
|Fallback notify entities|Used for reminders without recipients|
|Notification title|Default `Reminder`. `{text}` is the reminder, `{names}` the recipients|
|Notification message|Default `{text}`|
|Announcement on the satellites|Default `Reminder for {names}: {text}`|
|Word to join the last two names|Default `and`|
|Play a sound before the announcement|Default on|
|Triggers|When to check for reminders, by default every minute|

## Translation

### Dutch

```yaml
notification_title: Herinnering
notification_message: "{text}"
announcement: "Herinnering voor {names}: {text}"
and_word: en
```
