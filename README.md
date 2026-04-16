==================================================
Download AutoBaseController
===========================

Download here:
https://github.com/user-attachments/files/26771021/AutoBasesController_1.1.zip

==================================================
AutoBases Controller - Config Details
=====================================

==================================================
OVERVIEW
========

This bot uses your config to:

* connect to Discord
* read AutoBases webhook posts
* connect to your servers through RCON
* control spawning, retries, cooldowns, and cap enforcement

The config has 7 main sections:

* discord
* scheduler
* cap_enforcement
* maps
* rcon_servers
* logging
* live_feeds

==================================================
AUTOBASES PLUGIN REQUIREMENTS
=============================

This bot completely replaces AutoBases AutoSpawn.

You MUST disable AutoSpawn in your AutoBases config.

If AutoSpawn is enabled:

* The plugin will spawn bases
* The bot will also spawn bases
* This will cause duplicate spawns and broken behavior

==================================================
VERY IMPORTANT: HOW MAP KEYS WORK
=================================

Map keys (like island, rag, etc.) are NOT special values.

They are simply labels that must match between:

* maps
* rcon_servers

If they do NOT match → the bot will fail setup.

==================================================
DISCORD SECTION
===============

log_channel_id is used for BOTH modes:

* Logging mode → posts messages
* Live feed mode → shows persistent panels

==================================================
LOGGING MODE VS LIVE FEEDS (IMPORTANT)
======================================

This bot supports TWO display modes.

Controlled by:

"logging": {
"enabled": true/false
}

---

## LOGGING MODE (enabled = true)

* Posts individual log messages
* Creates full event history
* Can be spammy

Controlled by:

* log_spawn_attempts
* log_resyncs
* log_webhook_events
* log_spawn_failures
* log_spawn_timeouts

---

## LIVE FEED MODE (enabled = false)

* Uses persistent embeds (NO spam)
* Messages are edited, not reposted
* Clean dashboard-style display

Includes:

* Overview panel (all maps)
* Details panel (rotating pages)
* Recent activity panel

---

## RECOMMENDED DEFAULT

logging.enabled = false

==================================================
SCHEDULER SECTION
=================

spawn_retry_timeout_seconds

* Time before a spawn is considered delayed
* Recommended: 120
* Lower values can cause false timeout warnings

==================================================
LOGGING SECTION
===============

Example:

"logging": {
"enabled": false,
"log_spawn_attempts": false,
"log_resyncs": false,
"log_webhook_events": false,
"log_spawn_failures": false,
"log_spawn_timeouts": false
}

enabled

* TRUE → log messages
* FALSE → live feed panels (recommended)

==================================================
LIVE_FEEDS SECTION
==================

Used when logging.enabled = false

overview_enabled

* Shows summary of all maps

details_enabled

* Shows rotating detailed view

events_enabled

* Shows recent activity panel

details_maps_per_page

* Number of maps shown at once

events_limit

* Number of events stored

==================================================
WHY NOTHING IS SPAWNING
=======================

Most common reasons:

* Map is at max_active
* Cooldown still active
* Spawn interval not ready
* Webhook events missing

IMPORTANT:

If webhook messages are missing:

* Active bases may never clear
* Maps may stay permanently full
* Spawning will stop

Run:

!abstatus

==================================================
RECOMMENDED STARTER SETTINGS
============================

* mode = per_map
* startup_resync = true
* periodic_resync_minutes = 15
* spawn_retry_attempts = 3
* cap_enforcement.enabled = false
* logging.enabled = false

==================================================
FIRST RUN CHECKLIST
===================

1. Set Discord token
2. Set channel IDs
3. Set RCON info
4. Match map keys
5. Verify webhook channel
6. Start bot
7. Run !absetup
8. Fix issues
9. Run !abstatus

==================================================
COMMON PROBLEMS
===============

Nothing spawning

* Map at cap
* Cooldown active
* Interval not ready
* Webhook misconfigured
* RCON misconfigured

Wrong active count

* Missed webhook
* Run !abresync

Setup fails

* Check IDs
* Check token
* Check RCON
* Check key matching

Webhook not detected

* Check webhook channel
* Check permissions
* Confirm AutoBases is posting

==================================================
MAIN COMMANDS
=============

!abhelp
!absetup
!abstatus
!abresync
!abspawn <map> <group>
!abpause <map>
!abresume <map>
!abreload
!absetmode <per_map|cluster>
!abactive <map>
!abdestroy <map> <base_id>

==================================================
FINAL NOTE
==========

If something is not working:

!absetup

This will tell you exactly what is wrong.

