# Baby Tracker App — Feature Summary

## Overview

**Baby Tracker** is a lightweight, mobile-first web application designed to make it extremely fast and easy for two parents to track a baby's daily care.

The app focuses on four core activities:

* **Sleep**
* **Feeding**
* **Diaper changes**
* **Medicine**

It is designed to work especially well on an **iPhone**, including late at night when the goal is to open the app, record something in a few taps, and move on.

The app is hosted on **GitHub Pages** as a simple `index.html` file and uses **Google Apps Script + Google Sheets** as its shared backend.

This means there is:

* no traditional database
* no server to maintain
* no Rails or Node application
* no App Store installation
* no complicated login system
* no paid hosting requirement

Both parents can use the same app from separate phones while sharing the same live data.

---

# Core Architecture

The app uses a very simple structure:

```text
iPhone / Browser
       ↓
GitHub Pages
(index.html)
       ↓
Google Apps Script
       ↓
Google Sheet
("Baby Tracker")
```

The Google Sheet acts as the application's database.

Each recorded event is stored as a row containing information such as:

* event ID
* event type
* start time
* end time
* duration
* amount
* unit
* medicine name
* person who recorded it
* created timestamp
* additional event details

---

# Multi-Parent / Multi-Device Support

The app is designed to be used by two parents on separate phones.

During first-time setup, the user chooses:

* **Michael**
* **Alison**

The app remembers the selected person on that individual device.

Every event written to the Google Sheet records who entered it.

Example:

```text
Feed
5 oz
7:35 PM
Recorded by Michael
```

or:

```text
Diaper
Wet
8:02 PM
Recorded by Alison
```

Both phones share the same Google Sheet, so changes made from one phone become visible on the other.

---

# Sleep Tracking

## Start Sleep

Tapping the large **Start Sleep** button immediately records the baby's sleep start time.

The app creates an active Sleep record in Google Sheets as soon as the sleep begins.

This is important because the sleep session is not tied to one phone.

For example:

1. Michael starts the sleep timer.
2. The sleep record is immediately saved.
3. Alison opens the app on her phone later.
4. Her phone sees that the baby is currently sleeping.
5. Alison can end the sleep session herself.

---

## Live Sleep Timer

While the baby is sleeping, the main status area shows the live duration.

Example:

```text
SLEEPING NOW

2h 17m

Started 8:14 PM
```

The duration continues updating automatically.

---

# Awake Timer

When the baby is not sleeping, the app automatically calculates how long the baby has been awake since the end of the most recent sleep.

Example:

```text
AWAKE NOW

3h 27m

Awake since 1:02 PM
```

This makes it possible to answer questions such as:

> "How long has he been awake?"

without manually calculating the time since the last nap.

---

# Wake Window Planning

The app can also optionally use a **parent-defined wake-window target**.

For example, if the desired wake window is configured as:

```text
2 hours 30 minutes
```

the app can show:

```text
AWAKE NOW

2h 08m

Awake since 1:02 PM

Target nap around 3:32 PM
22m to target
```

The app does not prescribe developmental or medical wake windows.

Instead, the parents choose the target they want the app to use.

---

# Night Sleep vs. Daytime Naps

The app differentiates between:

* **Night Sleep**
* **Daytime Naps**

The default schedule is:

```text
6:00 AM – 6:00 PM = Nap

6:00 PM – 6:00 AM = Night Sleep
```

The classification is based on when the sleep session **starts**.

For example:

```text
5:45 PM – 7:00 PM
```

is considered a nap because it started before 6:00 PM.

The nap/night boundaries can be changed in Settings.

---

# Sleep Last Night

Instead of resetting sleep totals at midnight, the app calculates **Sleep Last Night** as a continuous overnight period.

For example:

```text
8:00 PM – 6:00 AM
```

is displayed as:

```text
10h
Sleep Last Night
```

rather than incorrectly showing only the portion after midnight.

This solves the common problem where a midnight reset makes overnight sleep totals misleading.

---

# Overnight Sleep Summary

The **Sleep Last Night** card can be opened for additional details.

The overnight summary can include:

```text
Sleep Last Night

Total asleep
9h 42m

Bedtime
8:12 PM

Morning wake
6:18 AM

Overnight wakeups
2

Time awake overnight
24m
```

The app can also show the individual sleep segments that contributed to the total.

For example:

```text
8:12 PM – 11:41 PM     3h 29m

11:57 PM – 2:38 AM     2h 41m

2:46 AM – 6:18 AM      3h 32m
```

---

# Daytime Nap Tracking

The app separately calculates the baby's daytime nap total.

Example:

```text
2h 17m
Naps Today

3 naps
```

This gives parents an immediate view of how much daytime sleep the baby has received without mixing it with overnight sleep.

---

# Individual Nap Breakdown

The app also displays individual naps.

Example:

```text
Today's Naps

8:14 AM – 8:52 AM
38m

10:47 AM – 11:39 AM
52m

1:22 PM – 2:09 PM
47m
```

If a nap is still happening:

```text
1:22 PM – Now
Snoozing
47m
```

---

# Manual Sleep Entry

If a sleep session was not started in real time, the parents can manually add it.

Example:

```text
Add Past Sleep

Sleep started:
1:15 PM

Sleep ended:
2:42 PM
```

The app calculates the duration automatically.

---

# "Still Snoozing" Sleep Entry

A manually entered or corrected sleep does not have to be finished.

The user can select:

```text
Still Snoozing
```

instead of entering an end time.

For example:

```text
Sleep started:
12:47 PM

Status:
Still Snoozing
```

The session becomes the active sleep session and the timer continues from the manually entered start time.

---

# Sleep Corrections

Any Sleep record can be edited.

A completed sleep can have both timestamps corrected:

```text
Sleep started
8:03 PM

Sleep ended
6:12 AM
```

The duration is recalculated automatically.

An active sleep can have its start time corrected without ending the sleep.

Example:

```text
Original:
9:03 PM

Corrected:
8:47 PM

Status:
Still Snoozing
```

The active timer then continues from 8:47 PM.

---

# Reopening a Completed Sleep

A completed Sleep record can also be changed back to:

```text
Still Snoozing
```

This removes the end timestamp and turns the sleep back into the currently active sleep session.

---

# Feeding Tracking

The large **Feed** card provides quick bottle tracking.

The user can enter how much breastmilk the baby consumed.

Example:

```text
Feed

3.5 oz
```

The amount can be adjusted using large:

```text
−    +
```

buttons.

---

# Feeding Timestamps

A feed can be recorded using:

### Now

The app timestamps the feed immediately.

or:

### Choose Time

The parent can manually enter the correct date and time.

This is useful if a feeding was not entered immediately.

---

# Feeding Memory

The app remembers the most recently used feeding amount on that phone.

For example, if the previous feeding was:

```text
4 oz
```

the next Feed modal can start with:

```text
4 oz
```

instead of requiring the amount to be entered from scratch.

---

# Last Feeding Information

The Feed card displays the most recent feeding.

Example:

```text
Feed

Last Feeding: 7:35 PM | 5 oz

2h 18m ago
```

This provides:

* the last feeding time
* the amount consumed
* how long ago the feeding occurred

---

# Rolling 24-Hour Feeding Totals

Feed totals use a **rolling 24-hour window** instead of resetting at midnight.

Example:

```text
18 oz
Milk · 24h

5
Feeds · 24h
```

This makes the information more meaningful at times such as 12:15 AM, when a calendar-day total would otherwise reset to nearly zero.

---

# Feed Detail View

Tapping the Milk or Feed summary can show the individual feedings from the last 24 hours.

Example:

```text
Milk · Last 24 Hours

Total Milk
16.5 oz

Feeds
4

7:35 PM     5 oz
4:12 PM     4 oz
1:06 PM     3.5 oz
9:42 AM     4 oz
```

---

# Diaper Tracking

The app includes a large dedicated **Diaper** card.

A diaper can be recorded as:

* **Wet**
* **Dirty**
* **Both**

The user can record the event:

* immediately
* or using a manually selected time

---

# Last Diaper Change

The Diaper card displays the most recent diaper change.

Example:

```text
Diaper

Last Diaper Change:
Wet | 7:55 PM

1h 36m ago
```

---

# Rolling 24-Hour Diaper Totals

Diaper totals use a rolling 24-hour period.

Example:

```text
7
Diapers · 24h

5 wet · 2 dirty
```

This provides more useful context than simply counting diaper changes since midnight.

---

# Last Dirty Diaper

The app can also surface the most recent dirty diaper separately.

For example:

```text
Last dirty:
3:22 PM
```

This makes it easier to track bowel movements without searching through the entire event history.

---

# Diaper Detail View

Tapping the diaper summary provides a breakdown of recent changes.

Example:

```text
Diapers · Last 24 Hours

7 total

5 wet
2 dirty

8:02 PM     Wet
5:48 PM     Both
3:22 PM     Dirty
1:07 PM     Wet
```

---

# Medicine Tracking

The large **Medicine** card allows parents to record medications.

Each record includes:

* medicine name
* dose
* unit
* timestamp
* person who entered it

Example:

```text
Tylenol
3.75 mL
7:45 PM
Michael
```

---

# Supported Medicine Units

The app supports units such as:

* mL
* mg
* tablet
* tsp

---

# Medicine Memory

The app remembers the most recently used:

* medicine name
* dose
* unit

on each phone.

This makes repeat medication entries much faster.

---

# Last Medicine Display

If medicine has been administered within the last 24 hours, the Medicine card shows it.

Example:

```text
Medicine

Last Meds:
Tylenol (3.75 mL) | 7:45 PM

2h 08m ago
```

If no medication has been recorded within the previous 24 hours, the subtitle remains blank.

This prevents old medication information from appearing as if it were recent.

---

# Recent Activity Timeline

The bottom of the app displays the most recent events.

Example:

```text
Recent Activity

☀ Nap
47m · 1:22 PM–2:09 PM

🍼 Feed
5 oz · Michael

◌ Wet diaper
Alison

＋ Tylenol
3.75 mL · Michael

☾ Night Sleep
3h 12m · 2:48 AM–6:00 AM
```

This gives parents a chronological snapshot of what has happened recently.

---

# Full Event Editing

Every event type can be edited after it has been entered.

## Feed

The user can change:

* amount
* date
* time

## Diaper

The user can change:

* Wet / Dirty / Both
* date
* time

## Medicine

The user can change:

* medicine name
* dose
* unit
* date
* time

## Sleep

The user can change:

* start time
* end time
* active/completed status

---

# Delete Events

Incorrect or accidental entries can be deleted.

The Delete option is available from the event's edit screen.

This means the Google Sheet does not have to be manually edited whenever somebody records something incorrectly.

---

# Undo

After recording an event, the app can briefly provide an **Undo** option.

Example:

```text
Feed saved · 5 oz

Undo
```

This is designed for situations such as:

* accidental button presses
* entering the wrong amount
* logging the wrong diaper type
* recording an event twice

---

# Offline / Poor-Connection Protection

The app includes a local pending-sync system.

When an event is recorded, it is temporarily stored on the phone before being confirmed by Google Sheets.

If the phone loses internet access, the app can show:

```text
2 changes saved on this phone
Waiting to sync
```

When the internet connection becomes available again, the app automatically attempts to sync the queued events.

---

# Duplicate Protection

Every event receives a unique ID.

This means that if:

* a request is delayed
* the browser retries
* the phone reconnects
* a pending event is resent

the backend can recognize the same event rather than creating duplicate rows.

---

# Sync Status

The interface can display when the app last successfully synchronized.

Example:

```text
Synced 7:42 PM
```

Settings can also show whether there are any locally pending changes.

---

# Current-State Synchronization

When the app is opened or brought back into view, it checks Google Sheets for the current state.

For example, if Alison starts a sleep session on her phone and Michael later opens his app, Michael's screen can immediately show:

```text
SLEEPING NOW

42m

Started 7:18 PM
```

The app also periodically refreshes while open.

---

# Daily vs. Rolling Totals

The app intentionally treats different measurements differently.

## Sleep Last Night

Based on the overnight sleep period.

## Naps Today

Based on daytime naps within the configured nap window.

## Milk

Rolling previous 24 hours.

## Feeds

Rolling previous 24 hours.

## Diapers

Rolling previous 24 hours.

This avoids the misleading behavior caused by resetting every measurement at midnight.

---

# Interactive Summary Cards

The top statistics are not simply labels.

They can be tapped to open more detailed information.

Examples include:

* Sleep Last Night
* Naps Today
* Milk · 24h
* Feeds · 24h
* Diapers · 24h

The homepage therefore remains simple while detailed information is still available when needed.

---

# Settings

Tapping the user's name opens the Settings area.

Settings can include:

## Current User

Choose:

* Michael
* Alison

## Wake Window Target

Configure the desired awake duration before the app estimates the next nap target.

## Nap Schedule

Configure which hours count as daytime naps.

Example:

```text
Nap start:
6:00 AM

Nap end:
6:00 PM
```

## Milk Units

Choose the preferred display unit:

* oz
* mL

## Sync Status

View:

* last synchronization
* pending offline events
* connection state

## Home Screen Information

View instructions/status related to installing the app on the phone's Home Screen.

---

# Mobile-First Design

The interface is specifically designed for phones.

It includes:

* large touch targets
* minimal typing
* high contrast
* dark nighttime interface
* large readable numbers
* rounded cards
* iPhone safe-area support
* bottom-sheet-style modals
* minimal navigation

The goal is to make the app usable one-handed and while sleep-deprived.

---

# Color-Coded Actions

The four primary actions each use a visually distinct colored card.

For example:

* **Sleep** — purple
* **Feed** — mint / green
* **Diaper** — warm amber / yellow
* **Medicine** — soft rose

This makes each activity easy to identify without carefully reading every label.

---

# iPhone Home Screen Support

The website is designed so it can be added directly to an iPhone Home Screen.

Using:

**Safari → Share → Add to Home Screen**

allows Baby Tracker to behave much more like a standalone application.

The app includes support for:

* standalone display behavior
* iPhone status-bar handling
* safe-area padding
* mobile viewport sizing
* Home Screen installation guidance

No App Store is required.

---

# Lightweight Technology

Despite all of these features, the front-end remains essentially one file:

```text
index.html
```

That file contains:

* HTML
* CSS
* JavaScript

There is no:

* React
* npm
* Node
* Rails
* webpack
* database server
* hosting server

The app remains intentionally small and fast.

---

# Google Sheets as the Data Store

All shared baby-care history is stored inside the **Baby Tracker** Google Sheet.

This has several benefits:

* data remains easy to inspect
* records can be manually corrected if ever necessary
* the parents retain direct access to the underlying data
* no proprietary database is required
* historical data can later be exported or analyzed

---

# Google Apps Script Backend

Google Apps Script acts as the API between GitHub Pages and Google Sheets.

It handles actions such as:

```text
startSleep
endSleep
addSleep
updateSleep

addFeed
updateFeed

addDiaper
updateDiaper

addMedicine
updateMedicine

deleteEvent

getState
```

It also performs calculations and validation before writing changes to the spreadsheet.

---

# Shared Family Access

The app uses a private **family code** to connect a device to the backend.

The code is entered once during device setup and stored locally on that phone.

This provides a lightweight layer of access control without requiring usernames and passwords every time the app is opened.

---

# The Overall Experience

The app is designed around two different levels of use.

## Quick Logging

At 3:00 AM:

```text
Open app
↓
Tap Feed
↓
Enter 4 oz
↓
Save
↓
Close app
```

or:

```text
Open app
↓
Tap Sleep
↓
Close app
```

## Deeper Review

When planning the day, the same application can answer questions such as:

* How long has the baby been awake?
* When should we roughly consider the next nap?
* How much did he sleep last night?
* How many times did he wake overnight?
* How much has he napped today?
* What were the individual nap lengths?
* How much milk has he consumed in the last 24 hours?
* When was the last feeding?
* How long ago was the last feeding?
* How many diapers has he had?
* How many were wet vs. dirty?
* When was the last dirty diaper?
* When was medicine last administered?
* Who recorded a particular event?
* Was an entry accidentally entered incorrectly?
* Are there any unsynced events waiting on the phone?

---

# Design Philosophy

The guiding principle of Baby Tracker is:

> **Keep routine actions extremely simple while making detailed information available only when it is needed.**

The homepage is therefore not intended to become a giant analytics dashboard.

Instead, it prioritizes:

1. **What is the baby doing right now?**
2. **How long has he been awake or asleep?**
3. **How did he sleep last night?**
4. **How much has he napped today?**
5. **When did he last eat, have a diaper changed, or receive medicine?**
6. **How quickly can a parent record the next event?**

The result is a lightweight shared baby tracker that combines **real-time logging, sleep scheduling context, rolling care totals, correction tools, offline protection, and detailed historical information** while remaining simple enough to operate in just a few taps.
