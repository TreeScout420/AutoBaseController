AutoBases Controller - Config Details

==================================================
OVERVIEW
==================================================

This bot uses your config to:
- connect to Discord
- read AutoBases webhook posts
- connect to your servers through RCON
- control spawning, retries, cooldowns, and cap enforcement

The config has 6 main sections:
- discord
- scheduler
- cap_enforcement
- maps
- rcon_servers
- logging


==================================================
VERY IMPORTANT: HOW MAP KEYS WORK
==================================================

The names like:

- island
- rag
- center
- ext

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
==================================================

Example:

"discord": {
  "token": "PUT_BOT_TOKEN_HERE",
  "admin_channel_id": 111111111111111111,
  "log_channel_id": 222222222222222222,
  "webhook_feed_channel_id": 333333333333333333
}

token
- Your Discord bot token
- This comes from the Discord Developer Portal
- Keep this private
- Never share this with anyone

admin_channel_id
- The channel where you will run bot commands
- Examples:
  - !absetup
  - !abstatus
  - !abspawn

log_channel_id
- The channel where the bot posts:
  - live dashboard
  - bot logs
  - cap enforcement notices
  - other system events

webhook_feed_channel_id
- This MUST be the channel where your AutoBases webhook is posting
- The bot MUST be able to read this channel
- If this is wrong, the bot will miss ACTIVE and COMPLETED events


==================================================
SCHEDULER SECTION
==================================================

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
- Controls how the bot schedules spawning
- Options:
  - per_map
  - cluster

per_map
- Each map runs on its own timer
- Best for most users
- Easiest to understand

cluster
- One shared timer chooses among eligible maps
- Uses map weights
- Better for a more global/pooled system

tick_seconds
- How often the bot loop runs
- 60 means the bot checks every 60 seconds

spawn_retry_timeout_seconds
- How long a spawn can sit before the bot considers it stuck
- Used for retry/recovery logic

startup_resync
- If true, the bot scans current active bases on startup
- Recommended: true

cluster_spawn_interval_minutes
- Only used if mode = cluster
- Controls how often the cluster scheduler tries a spawn

periodic_resync_minutes
- Failsafe resync timer
- Helps recover if webhook messages are missed
- Recommended to leave enabled

spawn_retry_attempts
- Number of times the bot retries failed spawns
- Helps when a spawn location is blocked


==================================================
CAP ENFORCEMENT SECTION
==================================================

Example:

"cap_enforcement": {
  "enabled": false,
  "mode": "destroy_newest"
}

enabled
- If true, the bot will automatically enforce max_active
- If a map has too many active bases, the bot will take action

mode
- Options:
  - destroy_newest
  - destroy_oldest
  - log_only

destroy_newest
- Keeps older bases
- Removes newest overflow bases first

destroy_oldest
- Keeps newer bases
- Removes oldest bases first

log_only
- Detects overflow
- Logs what would be removed
- Does NOT delete anything
- Best mode for testing


==================================================
MAPS SECTION
==================================================

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

Each entry in "maps" defines the spawn rules for one map.

plugin_map_name
- MUST match the AutoBases webhook/plugin map name exactly
- This is how the bot knows which webhook event belongs to which map

enabled
- true = bot uses this map
- false = bot ignores this map completely

max_active
- Maximum number of active bases allowed on this map

cooldown_minutes
- Time after a base completes before another can spawn on that map

spawn_interval_minutes
- Minimum time between spawns on that map
- Used in per_map mode

spawn_weight
- Used in cluster mode
- Higher number = map is chosen more often

spawn_groups
- Which groups the bot is allowed to spawn on that map

Two supported formats:

Simple:
"spawn_groups": ["Tier1", "Tier2"]

Weighted:
"spawn_groups": [
  { "name": "Tier1", "weight": 1 },
  { "name": "Tier2", "weight": 3 }
]

Group weight
- Higher weight = more likely to be chosen
- Example:
  - Tier1 weight 1
  - Tier2 weight 3
  - Tier2 is about 3x more likely


==================================================
RCON_SERVERS SECTION
==================================================

Example:

"rcon_servers": {
  "island": {
    "display_name": "The Island",
    "host": "127.0.0.1",
    "port": 27020,
    "password": "PUT_RCON_PASSWORD_HERE"
  }
}

This section tells the bot how to connect to each map server.

IMPORTANT:
The keys here MUST match the keys in "maps".

Example:

"maps": {
  "island": { ... }
}

"rcon_servers": {
  "island": { ... }
}

display_name
- Friendly name shown in bot messages and dashboard

host
- IP address or hostname for the server
- 127.0.0.1 means the bot is running on the same machine as the server

port
- RCON port for that map
- Must match your server's RCON configuration

password
- RCON password for that map
- Must match your server's RCON password exactly


==================================================
LOGGING SECTION
==================================================

Example:

"logging": {
  "log_spawn_attempts": false,
  "log_resyncs": false,
  "log_webhook_events": false
}

log_spawn_attempts
- Shows detailed spawn attempt logs
- Useful for debugging
- Can be noisy

log_resyncs
- Shows routine resync messages
- Usually best left off if you are using the live dashboard

log_webhook_events
- Shows raw webhook event logs
- Mostly useful for debugging webhook behavior


==================================================
RECOMMENDED STARTER SETTINGS
==================================================

For most users:

- mode = per_map
- startup_resync = true
- periodic_resync_minutes = 15
- spawn_retry_attempts = 3
- cap_enforcement.enabled = false at first
- logging = all false if using dashboard

After confirming everything works:
- enable cap_enforcement
- optionally use log_only first for testing


==================================================
FIRST RUN CHECKLIST
==================================================

1. Fill in Discord token
2. Fill in Discord channel IDs
3. Fill in RCON host, port, and password for each map
4. Make sure map keys match between:
   - maps
   - rcon_servers
5. Make sure webhook channel is correct
6. Start the bot
7. Run !absetup
8. Fix anything it reports
9. Run !abstatus


==================================================
COMMON PROBLEMS
==================================================

Nothing is spawning
- Map may be at cap
- Cooldown may still be active
- Interval may not be ready
- Webhook channel may be wrong
- RCON may be misconfigured

Bot shows wrong active base count
- Webhook may have been missed
- Periodic resync should correct this
- Run !abresync to force a refresh

!absetup fails
- Check Discord IDs
- Check token
- Check RCON host/port/password
- Check that map keys match between maps and rcon_servers

Webhook events are not being picked up
- Verify webhook_feed_channel_id
- Verify bot can read that channel
- Verify AutoBases is actually posting there


==================================================
MAIN COMMANDS
==================================================

!abhelp
- Brings up list of commands

!absetup
- Validates your setup and checks config, RCON, and Discord connections

!abstatus
- Shows current controller status including scheduler and map states

!abresync
- Forces a full refresh of active bases across all enabled maps

!abspawn <map> <group>
- Manually triggers a spawn attempt using a specific group

!abpause <map>
- Pauses automatic spawning on a specific map

!abresume <map>
- Resumes automatic spawning on a paused map

!abreload
- Reloads config.json without restarting the bot

!absetmode <per_map|cluster>
- Changes scheduler mode and saves it to config.json

!abactive <map>
- Displays all currently active bases on a map

!abdestroy <map> <base_id>
- Destroys a specific base by ID using RCON


==================================================
FINAL NOTE
==================================================

If something is not working, run:

!absetup

That command is the fastest way to find a config mistake.

[AutoBaseController_1.0.zip](https://github.com/user-attachments/files/26739589/AutoBaseController_1.0.zip)
