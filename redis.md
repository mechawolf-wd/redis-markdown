[----------------------------------------------------------------]
                              REDIS
[----------------------------------------------------------------]

*I* Redis is similar to MongoDB in that it is NoSQL and could be used for similar key/value cases.
*I* MongoDB is "disk-based" document store while Redis is "memory-based"
*I* Redis is used for caching
*I* Redis does NOT use Volatile cache. Redis persists data.

*I* Redis is designed to be accessed by trusted clients inside trusted environments.
*I* No expose policy would be great.

*I* Has tons of language support.

----------------------------
Redis Datatypes

- Strings
- Hashes
- Lists
- Sets
- Sorted Sets
- Bitmaps
- Hyperlogs
- Geospatial
- Indexes

*I* Redis does not server RAW data. (it is complex ya know...)
*I* Has no schemas and column names.

----------------------------
Traditional DB vs Redis

[#] Traditional:
- Query for full table

[#] Redis:
- No internal query engine

----------------------------
Security inside Redis

*I* IS DESIGNED TO BE USED BY TRUSTED USERS
*I* Do not allow any external access if possible because Redis does NOT SUPPORT data encryption.
*I* Deny access to main Redis port
*I* Redis is firewalled.
*I* Loopback can be used.

*E* DoS = Disable of Service

*IMPORTANT* Password is stored in plain text in the [redis.conf] file
*IMPORTANT* User must send {<AUTH>} command along with the password.

----------------------------
Installing Redis on Linux

# brew install redis
# brew install redis-server

----------------------------
@Redis's Cli

Gets in redis-cli
# redis-cli

*I* Or use redis-cli {<any-redis-cli-{command>}

<!-- ->> 127.0.0.1:6379 (looks like thiss) -->

*I* Would give you a type. (eg. (integer))

Checking connection
# ping

Echoes
# echo hello

Setting
# SET {<key>} {<value>}

Getting
# GET {<key>}

Incrementing (numeric command)
# INCR {<key>}

Incrementing by value (numeric command)
# INCRBY {<key>}

Decremeting (numeric command)
# DECR {<key>}

Decremeting by value (numeric command)
# DECRBY {<key>}

Checking existence
# EXISTS {<key>}

Deleting value
# DEL {<key>}

Writing into a file
# redis.cli {<COMMAND>} {<key>} > {<file>}

Monitoring Redis database state
You can think of it as cache-clear
# FLUSHALL

*I* :key is a syntax that you might use in the future.

When should it expire?
# EXPIRE <key> <seconds>

Checking how much time there is left
# TTL <key>

----------------------------
TTL timings

- (-2) means <key> is not set
- (-1) means <key> will never expire

----------------------------
Couple of CLI Commands pt. 1

--------
Key-Value pair commands

@CLI

Sets multiple keys (SET on steroids stands for Multiple SET)
*I* If VAR exists it is going to overwrite
*I* -NX commands will result in "(integer) 0" if value the already exists (1 otherwise).
# MSET <key> <value> <key> <value>

...variation is. However, this will not overwrite the vars.
# MSETNX

Multiple getter.
# MGET <key> <key>

Appending a string.
# APPEND <key> <string-to-append>

Substring of a string. (You can use negative offsets)
# GETRANGE <key> <start> <end>

Renaming.
# RENAME <key> <new-key>

Protected renaming. (Results in an error if key does not exist)
# RENAMENX

Getting and setting at the same time. (Returns the old value)
# GETSET <key>  <value>

Set key with a string value with a timeout.
# SETEX <key> <expiry-time> <value>

Set key with a string value with a timeout. (milliseconds)
# PSETEX <key> <expiry-time> <value>

... links with (milliseconds)
# PTTL <key>

Removes existing timeout on a key.
# PERSIST <key>

If the key already exists, it will not change
# SETNX <key> <value>

--------
Commands: SCAN & MATCH

SCAN - iterates the set of keys in the database.
     - used in production
     - takes cursor as a @param
     - iterates when cursor is set to 0
     - if you get 0 back from the server. Iteration is over.
     - with 0 -> first ten are printed.

# SCAN 0 COUNT 13

Matching something.
# SCAN 0 MATCH <value>
||
Starts with "k" in this case.
# SCAN 0 MATCH k*

Used with sets. Returns list of set members.
# SSCAN

Userd with hashes. Returns array of elements with a field and value.
# HSCAN

Used with sorted sets. Returns array of elements with associated score.s
# ZSCAN


--------
KEYS

- Should be avoided in production environments
- Can be taxing on the system

Get every key.
# KEYS *

Get keys by pattern.
# KEYS <pattern>

Get random key.
# RANDOMKEY

----------------
Configuration and Client

@CLI

Read configuration of running redis server.
# CONFIG GET <port>

Read all supported config params.
# CONFIG GET *

Match patterns?.
# CONFIG GET *max-*-entries* <- this is a pattern. or get an option like lua-time-limit

Setting configuration options at runtime.
# CONFIG SET <config-option> <value>

Getting information and statistics about the server.
# INFO

... choose from:
- server             - stats
- clients            - replication
- memory             - cpu
- cluster            - commandstats
- peristence         - keyspace
- all                - default

Resetting statistics.
# CONFIG RESETSTAT

Results in 6 sub-results.
# COMMAND

Returns details for a specific command.
# COMMAND INFO GET

Results in a count of available sever commands.
# COMMAND COUNT

--------
Client list

Listing active clients.
# CLIENT LIST

Assigning a name to a client.
# CLIENT SETNAME clientname

Return the name of the current client connection.
# CLIENT GETNAME

Closes up given connection.
# CLIENT KILL 127.0.0.1:<port-number>
||
# CLIENT KILL <id>
