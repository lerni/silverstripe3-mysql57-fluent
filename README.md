# SilverStripe 3.x Composer installable patch to mimic MySQL-Mode ANSI configuration as MySQL <5.7.x

- https://github.com/tractorcow/silverstripe-fluent/pull/415

## Requirements
* SilverStripe >3.6 <4

## Installation
Use [composer](https://getcomposer.org/):
`composer require lerni/silverstripe3-mysql57-fluent`

## Background
This fix is for SS 3.x only with SS 4 you sould update fluent to 4.1 or use a config as sunnysideup has in his module to fix the same issue on 4.x https://github.com/sunnysideup/silverstripe-mysql-5-7-fix

framework sets MySQL-Mode on every DB-request to ANSI-Mode
https://github.com/silverstripe/silverstripe-framework/blob/3/model/connect/MySQLDatabase.php#L45

As per MySQL 5.7 ANSI-Mode includs also `ONLY_FULL_GROUP_BY` and not only:
`REAL_AS_FLOAT,PIPES_AS_CONCAT,ANSI_QUOTES,IGNORE_SPACE`
https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html#sql-mode-combo

With the `ONLY_FULL_GROUP_BY` sorting a DataObject by a fluent translated field throws an error like:
`[User Error] Uncaught SS_DatabaseException: Couldn't run query: SELECT DISTINCT ... this is incompatible with DISTINCT`

This module patches MySQLDatabase.php to prevent the error above.