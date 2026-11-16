# Find My Phone (repeating critical alert)

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fwesleyericksonpmp%2Fhome-assistant%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fwesleyericksonpmp%2Ffind_my_phone.yaml)

Turn on a toggle and your phone rings through the Home Assistant Companion app until you find it. The alert repeats every 10 seconds, plays on silent (iOS Critical Alert), and stops when you tap **Turn Off** on the notification.

## What it does

- **Rings until stopped.** A notification goes out every few seconds while the toggle is on.
- **One alert, not a stack.** Each new alert replaces the last one on the phone.
- **Turn Off button.** Press and hold the notification and tap **Turn Off**. The phone never has to open the app.
- **Time limit.** The toggle turns itself off after 5 minutes if nobody stops it.
- **Cleans up.** The alert is cleared from the phone when it stops, however it stops.
- **Many phones.** Each phone gets its own Turn Off code and tag, built from the phone's name, so one person's button never stops another person's alert.

## Requirements

- Home Assistant 2024.10 or newer.
- The [Home Assistant Companion app](https://companion.home-assistant.io/) on each phone.
- iOS: allow **Critical Alerts** for the Home Assistant app (iOS Settings > Notifications > Home Assistant), so the alert plays on silent and Do Not Disturb.

## Setup

1. Import the blueprint with the button above.
2. Create one Toggle helper per phone: **Settings > Devices & services > Helpers > Create helper > Toggle**, for example "Find Wesley's iPhone".
3. Create one automation per phone from the blueprint. Pick the phone and its toggle.
4. Put the toggle on a dashboard, or turn it on from a voice assistant or another automation.

## Settings

| Setting | Default | What it does |
|---|---|---|
| Repeat every | 10 seconds | Time between alerts |
| Stop after | 5 minutes | Turns the toggle off if nobody stops it |
| Title | 📍 Find My Phone | Notification title |
| Message | Find your phone! | Notification text |
| Button label | Turn Off | Text on the notification button |

## How it works

The toggle is the switch. When it turns on, the automation sends the alert in a loop and checks the toggle before each one. The Turn Off button and the time limit both turn the toggle off. On its next check the loop sees the toggle is off, stops, and sends `clear_notification` with the same tag to remove the alert from the phone.

The automation runs in `parallel` mode, so the Turn Off button works while the alert loop is running.

## Limits

- **Tested on iOS only.** The Android settings (`channel: alarm_stream`, `priority: high`, `ttl: 0`) follow the Companion app docs, but I have not run them on an Android device yet.
- **Phone names must be unique.** The Turn Off code comes from the phone's name. Two phones with the same name would share a code.
- **The phone needs a connection.** A phone that is off, dead, or offline will not get the alert. The time limit still turns the toggle off.

## Changelog

- **2026-11-16:** First release.
