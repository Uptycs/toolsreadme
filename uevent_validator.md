# uevent_validator
A command-line client for validation of Uptycs JS events.

## Installation

If you already know how to install npm packages, then you can install it via npm:

You might need sudo on linux.

```
$ npm install uevent_validator --save
```

## Download the API key from uptyc. 

Following are the steps for downloading API key:
 1. Connect to you tenant using your username and password. Uptycs URL example: https://<tenant>.uptycs.io
 2. Got to configuration -> Users
 3. Go to your username click on Edit 
 4. Create and Dowload "User API key"

## Usage
```
$ uevent_validate --help
uevent_validate [command]

Commands:
  uevent_validate [options]  Options

Options:
  --version               Show version number  [boolean]
  --keyfile, -k           Uptycs JSON key file  [required]
  --query, -q             Single SQL query file contains SQL with LIMIT 1  [required]
  --script, -s            Event JScript  [required]
  --lookup_table, -t      LookupTable name
  --lookup_data_file, -d  LookupTable data file
  --domainsuffix          domain suffix default is .uptycs.io
  --help                  Show help  [boolean]tically switch to vertical output mode if the
                          result is wider than the terminal width.
  -t, --table             Display batch output in table format.
  --csv                   Display batch output in CSV format.
  --warn / --no-warn      Warn before running a destructive query.
  -e, --execute TEXT      Execute command and quit.
  -k, --keyfile TEXT      Uptycs Key file.
  --domainsuffix TEXT     Uptycs Domain Suffix like .uptycs.io
  --nossl                 do not verify ssl certificate
  --help                  Show this message and exit.
```

## Examples

### Example-1

1. Sample Query file: query_example.sql
```
SELECT * FROM processes where cmdline like '%grep%' and pid = 14406 LIMIT 1;
```

2. Sample Event JS script:
```
const events = [];
records.forEach(record => {
  const cmdline = record.columns['cmdline'];
  //const cmdline = record.columns['cmdline'];
  if (cmdline && cmdline.includes('grep')) {
    const desc = 'Test code';
    const event = {
      severity: 'high',
      description: desc,
      time: record.unixTime,
      key: 'process_name',
      value: record.columns['path'],
      pid: record.columns['pid'],
      cmdline: cmdline,
      login_name: record.columns['login_name'],
      uid: record.columns['uid'],
      gid: record.columns['gid'],
      parent_process: record.columns['ancestor_list'],
      cwd: record.columns['cwd']
    };
    events.push(event);
  }
});

return events;
```

3. Execute using following command:
```
uevent_validate -k ./authcode/allinone_admin.json \
                --query ./query_example.sql  \
                --script ./event_example.js
```

4. Output:
```
SQL
---------------------------------------------------------------------------
SELECT * FROM processes where cmdline like '%grep%' and pid = 14406 LIMIT 1


Record Array
-------------
[
  {
    columns: {
      upt_asset_id: '3cc4d442-e188-509b-af03-d492810dd13a',
      upt_time: '2019-09-17 19:27:50.000',
      upt_added: true,
      upt_epoch: 1568632054314,
      upt_counter: 1777,
      upt_hash: 'b38170bc-4697-5ed8-b3d2-3d0c35f223ac',
      pid: 14406,
      name: 'node',
      path: '/usr/local/Cellar/node/12.10.0/bin/node',
      cmdline: "node event_validator.js --file ../authcode/allinone_admin.json --query SELECT * FROM processes where cmdline like '%grep%' LIMIT 1 --event_script ./event_example.js",
      state: null,
      cwd: '/Users/vibhorkumar/Downloads/tools/restapicalls/uptycs_events_verifier',
      root: null,
      uid: 501,
      gid: 20,
      euid: 501,
      egid: 20,
      suid: 501,
      sgid: 20,
      on_disk: 1,
      wired_size: 0,
      resident_size: 0,
      total_size: 0,
      user_time: 0,
      system_time: 0,
      start_time: 0,
      parent: 4945,
      pgroup: 14406,
      threads: 0,
      nice: 0,
      upt_hostname: 'vibhors-macbook.local',
      disk_bytes_read: 0,
      disk_bytes_written: 0,
      is_elevated_token: 0,
      cgroup_namespace: null,
      ipc_namespace: null,
      mnt_namespace: null,
      net_namespace: null,
      pid_namespace: null,
      user_namespace: null,
      uts_namespace: null,
      upt_server_time: '2019-09-17 19:32:22.216',
      elapsed_time: 0,
      handle_count: 0,
      percent_processor_time: 0,
      upid: 0,
      uppid: 0,
      cpu_type: 0,
      cpu_subtype: 0,
      upt_asset_tags: [Object],
      upt_asset_group_id: '38196e03-d455-4e51-90e4-bc87b5332ca8',
      upt_asset_group_name: 'assets',
      upt_day: 20190917,
      upt_batch: 88
    }
  }
]

Events
-------
[
  {
    severity: 'high',
    description: 'Test code',
    time: undefined,
    key: 'process_name',
    value: '/usr/local/Cellar/node/12.10.0/bin/node',
    pid: 14406,
    cmdline: "node event_validator.js --file ../authcode/allinone_admin.json --query SELECT * FROM processes where cmdline like '%grep%' LIMIT 1 --event_script ./event_example.js",
    login_name: undefined,
    uid: 501,
    gid: 20,
    parent_process: undefined,
    cwd: '/Users/vibhorkumar/Downloads/tools/restapicalls/uptycs_events_verifier'
  }
]
```
**Note:** In case of error in Uptycs event JS, error will be printed on the scree.

### Example-2: Using lookup table

If a user wants to validate Uptycs event JScript with look up table, then they can use following example:

1. Create a lookup table data file as given below:
```
vim pid_array.json
[ 14406, 14407, 14408 ]
```
2. Lookup Table name: pid_arrray

3. Sample Lookup Uptycs Event JS: event_example2.js
```
const events = [];
records.forEach(record => {
  const data = getLookupRow('pid_array',record.columns['pid']);
  const cmdline = record.columns['cmdline'];
  if (data && cmdline && cmdline.includes('grep')) {
    const desc = 'Test code';
    const event = {
      severity: 'high',
      description: desc,
      time: record.unixTime,
      key: 'process_name',
      value: record.columns['path'],
      pid: record.columns['pid'],
      cmdline: cmdline,
      login_name: record.columns['login_name'],
      uid: record.columns['uid'],
      gid: record.columns['gid'],
      parent_process: record.columns['ancestor_list'],
      cwd: record.columns['cwd']
    };
    events.push(event);
  }
});

console.log(events);
return events;
```
3. Execute following command:
```

uevent_validate --keyfile=../authcode/allinone_admin.json \
                --query=./query_example.sql \
                --script=./event_example2.js \
                --lookup_table=pid_array \
                --lookup_data_file=./pid_array.json
```

4. Output:
```
SQL
---------------------------------------------------------------------------
SELECT * FROM processes where cmdline like '%grep%' and pid = 14406 LIMIT 1


Record Array
-------------
[
  {
    columns: {
      upt_asset_id: '3cc4d442-e188-509b-af03-d492810dd13a',
      upt_time: '2019-09-17 19:27:50.000',
      upt_added: true,
      upt_epoch: 1568632054314,
      upt_counter: 1777,
      upt_hash: 'b38170bc-4697-5ed8-b3d2-3d0c35f223ac',
      pid: 14406,
      name: 'node',
      path: '/usr/local/Cellar/node/12.10.0/bin/node',
      cmdline: "node event_validator.js --file ../authcode/allinone_admin.json --query SELECT * FROM processes where cmdline like '%grep%' LIMIT 1 --event_script ./event_example.js",
      state: null,
      cwd: '/Users/vibhorkumar/Downloads/tools/restapicalls/uptycs_events_verifier',
      root: null,
      uid: 501,
      gid: 20,
      euid: 501,
      egid: 20,
      suid: 501,
      sgid: 20,
      on_disk: 1,
      wired_size: 0,
      resident_size: 0,
      total_size: 0,
      user_time: 0,
      system_time: 0,
      start_time: 0,
      parent: 4945,
      pgroup: 14406,
      threads: 0,
      nice: 0,
      upt_hostname: 'vibhors-macbook.local',
      disk_bytes_read: 0,
      disk_bytes_written: 0,
      is_elevated_token: 0,
      cgroup_namespace: null,
      ipc_namespace: null,
      mnt_namespace: null,
      net_namespace: null,
      pid_namespace: null,
      user_namespace: null,
      uts_namespace: null,
      upt_server_time: '2019-09-17 19:32:22.216',
      elapsed_time: 0,
      handle_count: 0,
      percent_processor_time: 0,
      upid: 0,
      uppid: 0,
      cpu_type: 0,
      cpu_subtype: 0,
      upt_asset_tags: [Object],
      upt_asset_group_id: '38196e03-d455-4e51-90e4-bc87b5332ca8',
      upt_asset_group_name: 'assets',
      upt_day: 20190917,
      upt_batch: 88
    }
  }
]

Events
-------
[
  {
    severity: 'high',
    description: 'Test code',
    time: undefined,
    key: 'process_name',
    value: '/usr/local/Cellar/node/12.10.0/bin/node',
    pid: 14406,
    cmdline: "node event_validator.js --file ../authcode/allinone_admin.json --query SELECT * FROM processes where cmdline like '%grep%' LIMIT 1 --event_script ./event_example.js",
    login_name: undefined,
    uid: 501,
    gid: 20,
    parent_process: undefined,
    cwd: '/Users/vibhorkumar/Downloads/tools/restapicalls/uptycs_events_verifier'
  }
]
```

## Query file best practices

* Query file should contain SQL which can bring sample one row for testing purpose.
* Its highly recommended that you should use upt_day in the query for faster access of the rows
* Use LIMIT 1 at the end of the clause whenever possible
