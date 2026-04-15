==================================================
Download AutoBaseController
===========================

Download here: [AutoBaseController.zip](https://github.com/user-attachments/files/26740232/AutoBaseController.zip)

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

The config has 6 main sections:

* discord
* scheduler
* cap_enforcement
* maps
* rcon_servers
* logging

==================================================
AUTOBASES PLUGIN REQUIREMENTS
=============================

This bot completely replaces AutoBases AutoSpawn.

You MUST disable AutoSpawn in your AutoBases config.

Remove or disable any AutoSpawn settings.

If AutoSpawn is enabled:

* The plugin will spawn bases
* The bot will also spawn bases
* This will cause duplicate spawns and broken behavior

==================================================
VERY IMPORTANT: HOW MAP KEYS WORK
=================================

The names like:

* island
* rag
* center
* ext

are NOT special built-in values.

They are just labels used to link the config together.

Example:

"maps": {
"island": { ... }
}

"rcon_servers": {
"island": { ... }
}

The key MUST match in BOTH places.

You can rename them if you want, for example:

"maps": {
"my_island": { ... }
}

"rcon_servers": {
"my_island": { ... }
}

That is completely fine, as long as both sections use the exact same name.

If they do NOT match, the bot will fail setup/validation.

==================================================
DISCORD SECTION
===============

Example:

"discord": {
"token": "PUT_BOT_TOKEN_HERE",
"admin_channel_id": 111111111111111111,
"log_channel_id": 222222222222222222,
"webhook_feed_channel_id": 333333333333333333
}

token

* Your Discord bot token
* Keep this private

admin_channel_id

* Channel where you run bot commands
* Examples:

  * !absetup
  * !abstatus
  * !abspawn

log_channel_id

* Channel where the bot posts:

  * dashboard
  * logs
  * cap enforcement events

webhook_feed_channel_id

* MUST be the channel where AutoBases webhook posts
* Bot MUST have read access
* If wrong, bot will miss ACTIVE and COMPLETED events

==================================================
SCHEDULER SECTION
=================

Example:

"scheduler": {
"mode": "per_map",
"tick_seconds": 60,
"spawn_retry_timeout_seconds": 45,
"startup_resync": true,
"cluster_spawn_interval_minutes": 120,
"periodic_resync_minutes": 15,
"spawn_retry_attempts": 3
}

mode

* per_map → each map runs independently (recommended)
* cluster → shared timer across maps

tick_seconds

* How often the bot checks for work

spawn_retry_timeout_seconds

* How long before a spawn is considered stuck

startup_resync

* Sync active bases on startup (recommended: true)

cluster_spawn_interval_minutes

* Only used in cluster mode

periodic_resync_minutes

* Failsafe resync (recommended: keep enabled)

spawn_retry_attempts

* Retry attempts if spawn fails

==================================================
CAP ENFORCEMENT SECTION
=======================

Example:

"cap_enforcement": {
"enabled": false,
"mode": "destroy_newest"
}

enabled

* Automatically enforce max_active

mode

* destroy_newest → removes newest bases first
* destroy_oldest → removes oldest bases first
* log_only → detects but does not delete (best for testing)

==================================================
MAPS SECTION
============

Example:

"maps": {
"island": {
"plugin_map_name": "TheIsland_WP",
"enabled": true,
"max_active": 2,
"cooldown_minutes": 30,
"spawn_interval_minutes": 120,
"spawn_weight": 1,
"spawn_groups": [
{ "name": "Tier1", "weight": 1 },
{ "name": "Tier2", "weight": 1 }
]
}
}

plugin_map_name

* MUST match AutoBases webhook exactly

enabled

* true = enabled
* false = ignored

max_active

* Max active bases allowed

cooldown_minutes

* Delay after completion before next spawn

spawn_interval_minutes

* Minimum time between spawns

spawn_weight

* Used in cluster mode

spawn_groups

* Groups allowed to spawn

Simple:
"spawn_groups": ["Tier1", "Tier2"]

Weighted:
"spawn_groups": [
{ "name": "Tier1", "weight": 1 },
{ "name": "Tier2", "weight": 3 }
]

==================================================
RCON_SERVERS SECTION
====================

Example:

"rcon_servers": {
"island": {
"display_name": "The Island",
"host": "127.0.0.1",
"port": 27020,
"password": "PUT_RCON_PASSWORD_HERE"
}
}

IMPORTANT:
Keys MUST match "maps" section.

display_name

* Friendly name for logs

host

* Server IP (127.0.0.1 = local)

port

* RCON port

password

* Must match server RCON password

==================================================
LOGGING SECTION
===============

Example:

"logging": {
"log_spawn_attempts": false,
"log_resyncs": false,
"log_webhook_events": false
}

log_spawn_attempts

* Debug spawn attempts

log_resyncs

* Debug resync activity

log_webhook_events

* Debug webhook parsing

==================================================
WHY NOTHING IS SPAWNING
=======================

If the bot is running but nothing is spawning, the most common reasons are:

* The map is already at max_active
* No bases have been completed yet
* Cooldown is still active
* Spawn interval has not been reached
* Webhook events are missing

IMPORTANT:

If webhook messages are missing:

* Active bases may never clear
* Maps may stay permanently "full"
* Spawning will stop

Run:
!abstatus

This will show exactly what each map is waiting on.

==================================================
RECOMMENDED STARTER SETTINGS
============================

* mode = per_map
* startup_resync = true
* periodic_resync_minutes = 15
* spawn_retry_attempts = 3
* cap_enforcement.enabled = false (initially)
* logging = all false (use dashboard instead)

After testing:

* enable cap_enforcement
* optionally use log_only first

==================================================
FIRST RUN CHECKLIST
===================

1. Set Discord token
2. Set channel IDs
3. Set RCON host, port, password
4. Ensure map keys match
5. Verify webhook channel
6. Start bot
7. Run !absetup
8. Fix any issues
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
* Check RCON settings
* Check key matching

Webhook not detected

* Check webhook channel ID
* Check bot permissions
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

If something is not working, run:

!absetup

This is the fastest way to find configuration issues.
