# Libretime-Rotation

This is an implementation of "auto-dj" or automatic "rotation" for Libretime v4 and later. It has been tested working on Libretime 4.2.0. The purpose is to provide an "auto-dj" function, playing random selections during times where there is no scheduled show content.

If there is no show instance on the calendar in the next minute, it will create a block of random selections from the library that will closely fill the amount of time until the next scheduled event. A show called "ROTATION" containing this block of content will appear on the calendar. Everything else works as though a human had created it using the calendar interface.

Tracks with the genre of "ID" will be recognized as station ID stingers. When they will fit, they will be interspersed throughout the block every 5 or so songs. The script does a pretty good job of spinning up the content for the given block length, as long as you have a library of tracks with some decent length diversity (need some short tracks like 30 seconds, 1 minute, 2 minutes, etc.)

## Dependencies

- libretime
- php-cli

Nothing special if you already have libretime installed. It uses no third-party libraries.

## Installation

- Copy rotation.php to /usr/local/bin
- Add an entry to root's crontab as below:

### Crontab entry (Without logging)

Edit root's cron to run it every minute to see if there is a scheduling gap

`* * * * * /usr/bin/php /usr/local/bin/rotation.php > /dev/null`

### Crontab entry (With logging)

If you wish to have a log file, for example:

`* * * * * /usr/bin/php /usr/local/bin/rotation.php >> /root/rotation.log 2>&1`

No configuration is required. Necessary database and API configuration is read directly from the libretime config file at /etc/libretime/config.yml

## Usage

The script can be tested by firing it manually:
php rotation.php

If there is a need for a show, one will be created and populated.
If there is nothing to be done, it will simply say so and exit

## Notes

The Libretime library "genre" field is used to prevent certain library selections from being included as part of a rotation block. The following tags are excluded (case sensitive!):

- Show
- RotEx
- Rotex
- Podcast

The following tags are special:

- ID (Used to indicate a station ID or stinger that should be periodically inserted between groups of songs)

## Known issues

If playout is occuring when a rotation block is generated, there is a brief (.5 second or so) silence gap heard when Liquidsoap is restarted.

## Acknowlegements

Code making up the origins of this script were contributed by [Voisses Tech on the sourcefabric.org forum](https://forum.sourcefabric.org/discussion/18336/autodj-script-using-php-2-1-5-6-solution-you-were-waiting-on-no-ls_script-modification-need)
