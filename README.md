SOY is the contraction of Simple deplOY.

This tool is meant to replace huge deployment toolsets for simpler projects (especially PHP projects)

## Features
* Deploy files over SFTP.
* Deploy database schema
* Execute SSH commands
* Run SQL queries
* Multiconnections

## Usage
```
php soy.php <task-name>
```


## Example soy.php file

```php
<?php

include('soy/soy_starter.php');

Task('deploy', function() {
	Connection('local_database', 'mysql://username:pass@localhost/dbname');
	Connection('remote_database', 'mysql://username:pass@remotehost/dbname');

	Connection('local_folder', 'file:///local_folder');
	Connection('remote_folder', 'ssh://username:pass@remotehost/remote_folder');

	Run('files');
	Run('schema');
});

Task('files', function() {
	transfer('local_folder', 'remote_folder');
	select('remote_folder');
	bash("mv ".__('remote_folder.path')."/test_folder/test.txt ".__('remote_folder.path')."/test_folder/test_production.txt");
});

Task('schema', function() {
	transfer('local_database', 'remote_database');
	select('remote_database');
	sql_query("UPDATE page SET page_name = REPLACE(page_name, 'localhost', 'production')");
});

Start();

```
