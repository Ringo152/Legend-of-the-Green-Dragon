# Project TODOs

This file aggregates `TODO`, `FIXME`, and `XXX` comments found in the codebase.

## Critical / High Priority
*   **[modules/rules.php](file:///var/www/html/lotgd.io/modules/rules.php)**: Fix the template preference and cookie manipulation.
*   **[modules/settings.php](file:///var/www/html/lotgd.io/modules/settings.php)**: Fix the template preference and cookie manipulation.
*   **[lib/modules/settings.php](file:///var/www/html/lotgd.io/lib/modules/settings.php)**: Fix the cached db queries.

## Refactoring & Cleanup
*   **[lib/debuglog.php](file:///var/www/html/lotgd.io/lib/debuglog.php)**: Restructure this table to accept serialized data
*   **[lib/debuglog.php](file:///var/www/html/lotgd.io/lib/debuglog.php)**: Reduce the amount of arguments
*   **[lib/dbwrapper.php](file:///var/www/html/lotgd.io/lib/dbwrapper.php)**: Configure this in a commandline installer package instead of a weak setup
*   **[lib/pvpwarning.php](file:///var/www/html/lotgd.io/lib/pvpwarning.php)**: Remove the output from this function
*   **[lib/clan/membership.php](file:///var/www/html/lotgd.io/lib/clan/membership.php)**: Remove additional attributes, suggest a class for each table individually
*   **[lib/clan/create_clan.php](file:///var/www/html/lotgd.io/lib/clan/create_clan.php)**: Add default 'customsay' to the clan table, in lib/all_tables.php
*   **[lib/clan/detail.php](file:///var/www/html/lotgd.io/lib/clan/detail.php)**: Reformat the schema for clans, keys be renamed and not prefixed
*   **[lib/clan/detail.php](file:///var/www/html/lotgd.io/lib/clan/detail.php)**: Add a 'description_blocked' field for the Clans table
*   **[lib/clan/detail.php](file:///var/www/html/lotgd.io/lib/clan/detail.php)**: Add/Rename SU constants, because this one doesn't seem right.
*   **[lib/clan/detail.php](file:///var/www/html/lotgd.io/lib/clan/detail.php)**: Move this to its own file.
*   **[lib/pvplist.php](file:///var/www/html/lotgd.io/lib/pvplist.php)**: Remove these from this file.
*   **[lib/datacache.php](file:///var/www/html/lotgd.io/lib/datacache.php)**: Remove the usedatacache setting.
*   **[lib/datacache.php](file:///var/www/html/lotgd.io/lib/datacache.php)**: Change this name of this setting.
*   **[lib/systemmail.php](file:///var/www/html/lotgd.io/lib/systemmail.php)**: May want to add a modulehook to this since emailing is removed.
*   **[lib/pageparts.php](file:///var/www/html/lotgd.io/lib/pageparts.php)**: Move to cleanup, or a cronjob. Lags load times on pages ranomly.
*   **[configuration.php](file:///var/www/html/lotgd.io/configuration.php)**: Modularize this
*   **[village.php](file:///var/www/html/lotgd.io/village.php)**: 'collapse' module hooks could probably be removed when module handling is refactored
*   **[bio.php](file:///var/www/html/lotgd.io/bio.php)**: Remove this if statement.

## Improvements & Features
*   **[classes/Database.php](file:///var/www/html/lotgd.io/classes/Database.php)**: Implement cache to memory and file system.
*   **[lib/creatures.php](file:///var/www/html/lotgd.io/lib/creatures.php)**: Design a better algorithm for defense.
*   **[lib/addnews.php](file:///var/www/html/lotgd.io/lib/addnews.php)**: Change the date format from Y-m-d to Y-m-d H:i:s.
*   **[lib/all_tables.php](file:///var/www/html/lotgd.io/lib/all_tables.php)**: Create a MySQL trigger in the sql_updates.sql
*   **[modules/discord.php](file:///var/www/html/lotgd.io/modules/discord.php)**: Create the discord_commentary_channels for roleplay channels (live feeds of commentary)
*   **[modules/discord.php](file:///var/www/html/lotgd.io/modules/discord.php)**: Create the discord_commentary_message_map for mapping roleplay messages to discord messages

## Performance
*   **[lib/translator.php](file:///var/www/html/lotgd.io/lib/translator.php)**: Temporarily disable untranslated collection due to performance issues

## Documentation
*   **[lib/pageparts.php](file:///var/www/html/lotgd.io/lib/pageparts.php)**: Template Help
