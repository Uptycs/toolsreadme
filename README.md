# IUSQL
A command-line client for Uptycs that has auto-completion and syntax highlighting.

## Installation

If you already know how to install python packages, then you can install it via pip:

You might need sudo on linux.

```
$ pip install -U iusql
```
## Usage

$ iusql --help
Usage: iusql [OPTIONS] [DATABASE]

  A Uptycs terminal client with auto-completion and syntax highlighting.

  Examples:
    - iusql -k <uptycs keyfile> database

      where database = [global|realtime|audit]

Options:
  -V, --version           Output iusql's version.
  -D, --database TEXT     Database to use.
  -R, --prompt TEXT       Prompt format (Default: "\d> ").
  -l, --logfile FILENAME  Log every query and its results to a file.
  --iusqlrc PATH          Location of iusqlrc file.
  --auto-vertical-output  Automatically switch to vertical output mode if the
                          result is wider than the terminal width.
  -t, --table             Display batch output in table format.
  --csv                   Display batch output in CSV format.
  --warn / --no-warn      Warn before running a destructive query.
  -e, --execute TEXT      Execute command and quit.
  -k, --keyfile TEXT      Uptycs Key file.
  --domainsuffix TEXT     Uptycs Domain Suffix like .uptycs.io
  --nossl                 do not verify ssl certificate
  --help                  Show this message and exit.


A config file is automatically created at `~/.config/iusql/config` at first launch. See the file itself for a description of all available options.

# Connect and execute a query
```
vibhors-macbook:authcode vibhorkumar$  iusql  -k allinone_apikey.json global
Version: 2.0.2
Uptycs Security Solution
Uptysc Inc.
iusql allinone@global> SELECT upt_hostname, upt_time, days, hours, minutes, seconds from uptime limit 10;                                                                                                                  
+--------------------------------------------+-------------------------+------+-------+---------+---------+
| upt_hostname                               | upt_time                | days | hours | minutes | seconds |
+--------------------------------------------+-------------------------+------+-------+---------+---------+
| cenos7.c.perfect-entry-149521.internal     | 2019-04-09 22:55:08.000 | 54   | 9     | 29      | 15      |
| cenos7.c.perfect-entry-149521.internal     | 2019-04-09 22:55:08.000 | 54   | 10    | 31      | 26      |
| allinone                                   | 2019-04-09 16:58:50.000 | 55   | 16    | 58      | 0       |
| allinone                                   | 2019-04-09 16:58:50.000 | 55   | 17    | 56      | 33      |
| cenos7.c.perfect-entry-149521.internal     | 2019-04-10 10:18:54.000 | 54   | 20    | 53      | 0       |
| cenos7.c.perfect-entry-149521.internal     | 2019-04-10 10:18:54.000 | 54   | 21    | 55      | 11      |
| ip-172-31-84-85.ec2.internal               | 2019-04-10 09:54:35.000 | 52   | 12    | 49      | 42      |
| ip-172-31-84-85.ec2.internal               | 2019-04-10 09:54:35.000 | 52   | 13    | 55      | 42      |
| instance-4.c.perfect-entry-149521.internal | 2019-04-10 09:43:04.000 | 54   | 20    | 20      | 30      |
| instance-4.c.perfect-entry-149521.internal | 2019-04-10 09:43:04.000 | 54   | 21    | 20      | 42      |
+--------------------------------------------+-------------------------+------+-------+---------+---------+  
```

# Other shortcuts and command line options
```
iusql allinone@global> \?                                                                                                                                                                                                  
+------------+----------------------------+------------------------------------------------------------+
| Command    | Shortcut                   | Description                                                |
+------------+----------------------------+------------------------------------------------------------+
| .databases | .databases                 | List databases.                                            |
| .exit      | \q                         | Exit.                                                      |
| .mode      | \T                         | Change the table format used to output results.            |
| .once      | \o [-o] filename           | Append next result to an output file (overwrite using -o). |
| .open      | .open                      | Change to a new database.                                  |
| .schema    | .schema[+] [table]         | The complete schema for the database or a single table     |
| .status    | \s                         | Show current settings.                                     |
| .tables    | \dt[+] [table]             | List or describe tables.                                   |
| \G         | \G                         | Display current query results vertically.                  |
| \e         | \e                         | Edit command with editor (uses $EDITOR).                   |
| \f         | \f [name [args..]]         | List or execute favorite queries.                          |
| \fd        | \fd [name]                 | Delete a favorite query.                                   |
| \fs        | \fs name query             | Save a favorite query.                                     |
| help       | \?                         | Show this help.                                            |
| nopager    | \n                         | Disable pager, print to stdout.                            |
| notee      | notee                      | Stop writing results to an output file.                    |
| pager      | \P [command]               | Set PAGER. Print the query results via PAGER.              |
| prompt     | \R                         | Change prompt format.                                      |
| quit       | \q                         | Quit.                                                      |
| rehash     | \#                         | Refresh auto-completions.                                  |
| source     | \. filename                | Execute commands from file.                                |
| system     | system [command]           | Execute a system shell commmand.                           |
| tee        | tee [-o] filename          | Append all results to an output file (overwrite using -o). |
| watch      | watch [seconds] [-c] query | Executes the query every [seconds] seconds (by default 5). |
+------------+----------------------------+------------------------------------------------------------+
```

