
# HestiaCP PHP API

## How to use

1) Installation

```sh

$ composer require ponasromas/hestiacp-api

```

2) Create Client

a) Easy way

```php

use HestiaCP\Client;

// Easy way to create Client

// Using API Key
$client = Client::simpleFactory('https://someHost', 'API_Key');

```

b) For some reasons (more hosts, etc) you may need create objects alone

```php

use HestiaCP\Client;
use HestiaCP\Authorization\Credentials;
use HestiaCP\Authorization\Host;

// You can choose to use an API Key or username and password

// API Key
$credentials = new Credentials('API_Key');

$host = new Host('https://someHost', $credentials);

$client = new Client($host);

```

3) Usage

```php

// verify login
$client->testAuthorization(); // bool

```

You can simply send one of prepared commands (or you can write own command - must implements `HestiaCP\Command\ICommand` )

```php

$command = new SomeCommand();

$response = $client->send($command);


echo $response->getResponseText();

```

Or you can use prepared modules

a) user module

```php

$userModule = $client->getModuleUser();


$userModule->list(); // returns all users with data

$userModule->detail('admin'); // returns selected user with data

$userModule->changePassword('admin', 'otherPa$$word');

$userModule->add('other_user', 'pa$$word', 'some@email.com');

$userModule->suspend('other_user');

$userModule->unsuspend('other_user');

$userModule->delete('other_user');

```

b) web module

```php

$webModule = $client->getModuleWeb('admin'); // web module needs user


$webModule->listDomains();

$webModule->addDomain('domain.com');

$webModule->addDomainLetsEncrypt('domain.com', 'www.domain.com'); // needs longer timeout

$webModule->deleteDomainLetsEncrypt('domain.com');

$webModule->addDomainFtp('domain.com', 'test', 'pa$$word');

$webModule->changeDomainFtpPassword('domain.com', 'admin_test', 'otherPa$$word');

$webModule->changeDomainFtpPath('domain.com', 'admin_test', 'path/other');

$webModule->deleteDomainFtp('domain.com', 'admin_test');

$webModule->suspendDomain('domain.com');

$webModule->unsuspendDomain('domain.com');

$webModule->deleteDomain('domain.com');

```

c) mail module

```php

$mailModule = $client->getModuleMail('admin'); // mail module needs user


$mailModule->listDomains(); // returns mail domains from selected user

$mailModule->addDomain('domain.com'); // add domain

$mailModule->listAccounts('domain.com'); // returns accounts from selected user and domain

$mailModule->listDomainDkim('domain.com');

$mailModule->listDomainDkimDns('domain.com');

$mailModule->addAccount('domain.com', 'info', 'pa$$word'); // add info@domain.com account

$mailModule->changeAccountPassword('domain.com', 'info', 'otherPa$$word'); // change info@domain.com password

$mailModule->suspendAccount('domain.com', 'info'); // suspend info@domain.com account

$mailModule->unsuspendAccount('domain.com', 'info'); // unsuspend info@domain.com account

$mailModule->deleteAccount('domain.com', 'info');

$mailModule->suspendDomain('domain.com');

$mailModule->unsuspendDomain('domain.com');

$mailModule->deleteDomain('domain.com');

```

d) db module

```php

$dbModule = $client->getModuleDatabase('admin');


$dbModule->add('database', 'dbuser', 'dbpass');

$dbModule->delete('admin_database');

$dbModule->deleteDatabases();

$dbModule->listDatabase('database');

$dbModule->listDatabases();


// todo

// ... etc

```

e) cron module

```php

$cronModule = $client->getModuleCron();


// todo

// ... etc

```

f) backup module

```php

$backupModule = $client->getModuleBackup('admin'); // backup module needs user

$backupModule->backup(); // create a new backup

$backupModule->delete('admin.2021-10-13_18-12-53.tar'); // delete an user backup

$backupModule->deleteExclusions(); // delete all backup exclusions

$backupModule->listBackups(); // returns the backups list

$backupModule->listBackup('admin.2021-10-13_18-12-53.tar'); // returns backup parameters list

$backupModule->listBackupExclusions(); // returns the backup exclusions list

```
g) DNS module

Class self-documented.
