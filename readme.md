# Synapse Scripts

## synapse_janitor.sql
Cleans a synapse Postgres database of deleted messages and abandoned rooms.

### Deleted Messages
If a message was sent in matrix and is then deleted (or *redacted* in technical parlance) it is not displayed but it is not removed from the server either. This script removes it.

### Abandoned Rooms
If a room has been joined by someone on your homeserver but currently nobody on your homeserver is using it, this script will remove all traces of that room. **NOTE**: It will cause the room to disappear from people's "historical" section of old rooms.
This can improve performance significantly.

### Using this script
This script should be run from the command line while synapse server is stopped.

```
psql -Umatrix synapse < synapse_janitor.sql
```

## synapse_dumper.sh
Allows easy backup of a matrix postgres database by dumping it to a compressed file.

## synapse_startloop.sh
Script which starts up synapse if it is not running. **NOTE**: This is configured for a matrix server which has the synchrotron
worker enabled, if you do not use the synchrotron, just comment the block at the bottom.

## Using these scripts

The startloop and the dumper are appropriate for use in crontab:

```
* * * * * /home/matrix/synapse_scripts/synapse_startloop.sh 2>&1 >>/home/matrix/synapse_startloop.log
0 3 * * * /home/matrix/synapse_scripts/synapse_dumper.sh 2>&1 >>/home/matrix/synapse_dumper.log
```