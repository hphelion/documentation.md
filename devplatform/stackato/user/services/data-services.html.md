---
layout: default-devplatform
permalink: /als/v1/user/services/data-services/
---

Data Services[](#index-0 "Permalink to this headline")
=======================================================

Intro[](#intro "Permalink to this headline")
---------------------------------------------

Application Lifecycle Service includes a number of data services which can be bound to the
applications you deploy. These include several databases (PostgreSQL,
MySQL, MongoDB, Redis), the RabbitMQ messaging service, a [*persistent
file system*](/als/v1/user/services/filesystem/#persistent-file-system) service and
[*Memcached*](/als/v1/user/services/memcached/#memcached).

Configuring Application Lifecycle Service Data Services[](#configuring-helion-data-services "Permalink to this headline")
-------------------------------------------------------------------------------------------------------

The data services your application requires need to be specified at the
time your app is pushed to the Application Lifecycle Service server. This can be done in a
number of ways:

1.  Specifying the required services in the *stackato.yml* file.
2.  Configuring services during the `push` process.
3.  Configuring services manually.

If you would like to use an external database system, see [*Using
External Database Services*](#database-external).

### Using stackato.yml[](#using-stackato-yml "Permalink to this headline")

The stackato.yml file can hold a lot of application specific details
that tell the Application Lifecycle Service Client what to do without having to enter them
when you run `helion push`. For complete details
for the stackato.yml file, please see [*Configuration With
stackato.yml*](/als/v1/user/deploy/stackatoyml/#stackato-yml).

A simple example:

    name: cirrus
    mem: 256M
    instances: 2
    services:
        cirrusdb: mysql

This tells the Application Lifecycle Service Client to request a MySQL database called
`cirrusdb`. Possible service types are:

-   [*filesystem*](/als/v1/user/reference/glossary/#term-filesystem)
-   [*memcached*](/als/v1/user/reference/glossary/#term-memcached)
-   [*mongodb*](/als/v1/user/reference/glossary/#term-mongodb)
-   [*mysql*](/als/v1/user/reference/glossary/#term-mysql)
-   [*postgresql*](/als/v1/user/reference/glossary/#term-postgresql)
-   [*rabbitmq*](/als/v1/user/reference/glossary/#term-rabbitmq)
-   [*redis*](/als/v1/user/reference/glossary/#term-redis)

To access the data services once they've been created, see [*Accessing
Configured Database Services*](#database-accessing).

### Using helion push[](#using-helion-push "Permalink to this headline")

If you do not specify services in the stackato.yml file, you will be
prompted to create one during the push process. Should you want to set
up a database service, enter "y" when asked, and follow the prompts:

    $ helion push

    ...
    Would you like to bind any services to 'cirrus' ?  [yN]: y
    The following system services are available
    1. mongodb
    2. mysql
    3. postgresql
    4. redis
    5. <None of the above>
    Please select one you wish to provision: 2
    Specify the name of the service [mysql-18cab]: cirrusdb
    Creating Service: OK
    Binding Service: OK
    ...

In order to ensure the correct services are configured each time the app
is pushed, your services should be listed in the stackato.yml file.

### Creating and Binding Services[](#creating-and-binding-services "Permalink to this headline")

It is possible to create services and bind them to an app after they are
pushed to the Application Lifecycle Service server. There are two ways to do this:

**helion create-service \<service\> \<name\> \<app\>**
:   This combines all parameters into a single command.
    `service` is the type of service you want to
    create (mysql, redis, postgresql, mongodb). `name`{.docutils
    .literal} is the name you want to assign to the service.
    `app` is the name of the application the service
    is to be bound to.

        $ helion create-service mysql ordersdb myapp
        Creating Service: OK
        Binding Service: OK
        Stopping Application [myapp]: OK
        Staging Application [myapp]: OK
        Starting Application [myapp]: OK

        $ helion apps

        +-------------+---+-------------+---------------------------+----------------+
        | Application | # | Health      | URLS                      | Services       |
        +-------------+---+-------------+---------------------------+----------------+
        | myapp       | 1 | RUNNING     | myapp.helion-xxxx.local | ordersdb       |
        +-------------+---+-------------+---------------------------+----------------+

**create-service \<service\> \<name\>**

**bind-service \<servicename\> \<app\>**
:   These two commands do the same thing as if all three parameters were
    passed using `create-service`, but it allows the
    flexibility of creating and perhaps configuring the service before
    binding it.

    `service` is the type of service you want to
    create (mysql, redis, postgresql, mongodb). `name`{.docutils
    .literal} is the name you want to assign to the service.
    `servicename` is the name assigned during the
    `create-service` command. `app`{.docutils
    .literal} is the name of the application the service is to be bound
    to.

        $ helion create-service mysql customerdb
        Creating Service: OK

        $ helion bind-service customerdb myapp
        Binding Service: OK
        Stopping Application [myapp]: OK
        Staging Application [myapp]: OK
        Starting Application [myapp]: OK

        $ helion apps

        +-------------+---+---------+---------------------------+-----------------------+
        | Application | # | Health  | URLS                      | Services              |
        +-------------+---+---------+---------------------------+-----------------------+
        | myapp       | 1 | RUNNING | myapp.helion-xxxx.local | ordersdb, customerdb  |
        +-------------+---+---------+---------------------------+-----------------------+

For further information on the commands for manging services, please see
the [*helion services*](/als/v1/user/reference/client-ref/#command-services)
command reference.

**Note**

To remotely check the settings and credentials of any Application Lifecycle Service service,
use the [*helion
service*](/als/v1/user/reference/client-ref/#command-services) command.

Using Database Services[](#using-database-services "Permalink to this headline")
---------------------------------------------------------------------------------

When you bind a database service to an application running in Application Lifecycle Service,
[*environment
variables*](/als/v1/user/reference/environment/#environment-variables)
containing that service's host, port, and credentials are added to the
application container. You can use these environment variables in your
code to connect to the service, rather than hard coding the details.

Examples of how to parse and use these variables can be found in the
[*Language Specific
Deployment*](/als/v1/user/deploy/#language-specific-deploy) section.

### DATABASE\_URL[](#database-url "Permalink to this headline")

**If only one relational database service** is bound to an application,
use the DATABASE\_URL environment variable. It contains the connection
string for the bound database in the following format:

    protocol://username:password@host:port/database_name

For example, a DATABASE\_URL for a PostgreSQL service would look like
this:

    postgres://u65b0afbc8f8f4a1192b73e8d0eb38a24:p9eb83c11c59c4bcabfa475a4871e9242@192.168.69.117:5432/da17e48ddc82848499cb387bc65f5d4f9

The "protocol" portion specifies the type of database. For example:

-   mysql://
-   postgresql://

**Note**

The "database name" portion of the URL is the *actual* database name
(e.g. "da17e48ddc82848499cb387bc65f5d4f9"), not the user-specific
service name set during deployment/service creation (e.g. "myapp-db").

### Database-Specific URLs[](#database-specific-urls "Permalink to this headline")

**If a non-relational data service type** is bound to the application,
use the corresponding named environment variable:

-   MONGODB\_URL
-   REDIS\_URL
-   RABBITMQ\_URL

**If more than one relational database service type** is bound to the
application (e.g. MySQL and PostgreSQL), the DATABASE\_URL variable will
not be set but the following database-specific variables will:

-   MYSQL\_URL
-   POSTGRESQL\_URL
-   ORACLE\_URL (with Oracle Database add-on)

These have the same format as DATABASE\_URL.

**If more than one database of the same type** is bound to the
application (e.g. two MongoDB services), none of the URL formatted
environment variables will be available. Use STACKATO\_SERVICES or
VCAP\_SERVICES instead.

### STACKATO\_SERVICES[](#helion-services "Permalink to this headline")

Contains a JSON string listing the credentials for all bound services,
grouped by service name. For example:

    {
            "postdb": {
                    "name": "d4854a20e5854464891dbd56c08c440d9",
                    "host": "192.168.0.112",
                    "hostname": "192.168.0.112",
                    "port": 5432,
                    "user": "u74499595373c4bea84be2a87c2089101",
                    "username": "u74499595373c4bea84be2a87c2089101",
                    "password": "pdbbe19398c5a4463bba0644f7798f1f1"
            },
            "mydb": {
                    "name": "d0a60c0be931f4982bbef153f993237bc",
                    "hostname": "192.168.0.112",
                    "host": "192.168.0.112",
                    "port": 3306,
                    "user": "u93Mm8XmGXQ9R",
                    "username": "u93Mm8XmGXQ9R",
                    "password": "p8LwNeQXMrNzi"
            }
    }

### VCAP\_SERVICES[](#vcap-services "Permalink to this headline")

Contains a JSON string listing the credentials for all bound services,
grouped by service type. For example:

    {
            "mysql": [
                    {
                            "name": "mydb",
                            "label": "mysql-5.5",
                            "plan": "free",
                            "tags": [
                                    "mysql",
                                    "mysql-5.5",
                                    "relational"
                            ],
                            "credentials": {
                                    "name": "d0a60c0be931f4982bbef153f993237bc",
                                    "hostname": "192.168.0.112",
                                    "host": "192.168.0.112",
                                    "port": 3306,
                                    "user": "u93Mm8XmGXQ9R",
                                    "username": "u93Mm8XmGXQ9R",
                                    "password": "p8LwNeQXMrNzi"
                            }
                    }
            ]
    }

This variable contains some additional meta-information, and can be used
for compatibility with Cloud Foundry.

**Note**

VCAP\_SERVICES variables in Application Lifecycle Service v2.2 and later use non-versioned
service names The version number remains in 'label' key.

Using External Databases[](#using-external-databases "Permalink to this headline")
-----------------------------------------------------------------------------------

Applications running in Application Lifecycle Service can use external databases by
hard-coding the host and credentials, or by specifying the them in a
custom environment variable.

Hard-coded Database Connections[](#hard-coded-database-connections "Permalink to this headline")
-------------------------------------------------------------------------------------------------

Applications which write database connection details during staging
rather than taking them from environment variables at run time, must be
re-staged (e.g. redeployed or updated) to pick up the new service
location and credentials. Restarting the application will not
automatically force restaging.

Accessing Database Services[](#accessing-database-services "Permalink to this headline")
-----------------------------------------------------------------------------------------

You may need to connect to a database service directly for purposes of
initial database setup, modifying fields, running queries, or doing
backups. These operations can be done using the `dbshell`{.docutils
.literal} (preferred) or `tunnel` commands.

### Using dbshell[](#using-dbshell "Permalink to this headline")

The `helion dbshell` command creates an SSH tunnel
to database services. To open an interactive shell to a service:

    $ helion dbshell <application_name> <service_name>

The command will automatically open the appropriate database client for
the database you're connecting to, provided that client is installed on
the local system.

It is also available inside application containers, providing a quick
way to import data from dump files, or setting up schemas. For example,
to import data from file in an application directory, you could use a
hook in *stackato.yml* such as:

    hooks:
      post-staging:
        - dbshell < setup/sample-data.sql

### Using Tunnel[](#using-tunnel "Permalink to this headline")

The `helion tunnel` command is an alternative
method for accessing database services. The command creates a small Ruby
application which proxies database requests over HTTP. This is the
standard method for database access in Cloud Foundry, but tends to be
slower than using `dbshell`:

To create or use a tunnel:

    $ helion tunnel <servicename>

Depending on the service you are connecting to, a list of options will
be provided. Here is an example of connecting to a MySQL service:

    $ helion tunnel mydb

    Getting tunnel url: OK, at https://tunnel-xxxxx.helion-xxxx.local
    Getting tunnel connection info: OK

    Service connection info:
    +----------+-----------------------------------+
    | Key      | Value                             |
    +----------+-----------------------------------+
    | username | uT9efVVFCk                        |
    | password | pHFitpIU1z                        |
    | name     | d5eb2468f70ef4997b1514da1972      |
    +----------+-----------------------------------+

    1. none
    2. mysql
    3. mysqldump
    Which client would you like to start?

For simple command line access, select option **2. mysql**.

To get a dump of the entire database, select option **3. mysqldump**.
You will be prompted to enter a path to where the dump will be saved to.

If you want to connect with a database viewer, or run multiple commands
from the command line, passing in SQL files, select option **1. none**.
This will set up a port for you to connect with locally:

    1. none
    2. mysql
    3. mysqldump

    Which client would you like to start? **none**

    Starting tunnel to remarks on port 10000.
    Open another shell to run command-line clients or
    use a UI tool to connect using the displayed information.
    Press Ctrl-C to exit...

You how have all the information you need to access the data. Notice the
"Service connection info" box above that tells you your username,
password, and the database name.

Open a new command line window. You can connect to the MySQL database
directly with:

    $ mysql --protocol=TCP --host=localhost --port=10000 --user=<user> --password=<password> <name>

    example:

    $ mysql --protocol=TCP --host=localhost --port=10000 --user=uT9efVVFCk --password=pHFitpIU1z d5eb2468f70ef4997b1514da1972

To import an SQL file, call the same command, and pipe in the file:

    $ mysql --protocol=TCP --host=localhost --port=10000 --user=<user> --password=<pass> <name> < mydatabase.sql

To pull a dump of all databases:

    $ mysqldump -A --protocol=TCP --port=10000 --host=localhost --user=<user> --password=<pass>

### Pre-populating a database while pushing an app[](#pre-populating-a-database-while-pushing-an-app "Permalink to this headline")

When a database needs to be populated with data the first time it is
run, it can be done by the use of a hook during the staging process.
This can be accomplished in two steps.

First, create a script file in the app's root directory that uses the
same data source variables from STACKATO\_SERVICES as the ones being
used in the app. This file will open a connection to the database,
create tables, and insert records as necessary, as in this Perl example:

    use strict;
    use warnings;

    use DBI;
    use DBD::mysql;
    use JSON "decode_json";

    my $services = decode_json($ENV{STACKATO_SERVICES});
    my $credentials = $services->{mydb};

    my $dbh = DBI->connect("DBI:mysql:database=$credentials->{name};hostname=$credentials->{hostname};port=$credentials->{port};",
                           $credentials->{'user'}, $credentials->{'password'})
        or die "Unable to connect: $DBI::errstr\n";

    my $sql_init =
        'CREATE TABLE customers (
                        id INT(11) AUTO_INCREMENT PRIMARY KEY,
                        customername TEXT,
                        created DATETIME
                );
        ';
    $dbh->do($sql_init);

    $sql_init =
                'INSERT INTO customers
                        (customername, created)
                VALUES
                        ("John Doe", now()),
                        ("Sarah Smith", now());
        ';
    $dbh->do($sql_init);

    $dbh->disconnect;

Next, modify your *stackato.yml* file to make use of the
`post-staging` hook which will execute a command to
run the script:

    name: customertracker
    services:
      mysql: customerdb
    hooks:
      post-staging: perl preload.pl

With those changes, the data from your script will be executed after the
staging process is complete but before the app starts to run.

### Backing up a MySQL database[](#backing-up-a-mysql-database "Permalink to this headline")

#### Using helion run[](#using-helion-run "Permalink to this headline")

To export a MySQL database, use the `helion run`
command to remotely execute the dbexport tool:

``` {.literal-block}
$ helion run [application-name] dbexport service-name > dumpfile.sql
```

This will run a `dbexport` of the named data service
remotely and direct the output to a local file. If run from a directory
containing the stackato.yml file, the application name may be omitted.

#### Using helion tunnel[](#using-helion-tunnel "Permalink to this headline")

**Note**

This method of database backup is available for compatibility with Cloud
Foundry. It tends to be slower than using `helion run ...`{.docutils
.literal}.

To back up a MySQL database, use the [*tunnel*](#database-tunnel)
command to make a connection to the server and export the data using
`mysqldump`.

Use the `tunnel` command to access the service (in
this example a MySQL database named `customerdb`):

    $ helion tunnel customerdb

    Password: ********
    Getting tunnel url: OK, at https://tunnel-xxxxx.helion-xxxx.local
    Getting tunnel connection info: OK

    Service connection info:
    +----------+-----------------------------------+
    | Key      | Value                             |
    +----------+-----------------------------------+
    | username | uT9efVVFCk                        |
    | password | pHFitpIU1z                        |
    | name     | d5eb2468f70ef4997b1514da1972      |
    +----------+-----------------------------------+

    1. none
    2. mysql
    3. mysqldump
    Which client would you like to start?

Select option **3. mysqldump**. You will be prompted to enter a path to
where the dump will be saved.

See the [*tunnel*](#database-tunnel) command documentation for other
ways of accessing a MySQL database. See [*Importing a MySQL
database*](#bestpractices-importing-mysql) for details on importing a
file created by mysqldump into an existing MySQL database service.

### Importing a MySQL database[](#importing-a-mysql-database "Permalink to this headline")

#### Using helion run[](#id2 "Permalink to this headline")

To import a MySQL database, use the `helion dbshell`{.docutils
.literal} command:

    $ helion dbshell [application name] [service name] < dumpfile.sql

This command redirects the contents of a local database dump file to the
appropriate database client running in the application instance (i.e.
equivalent to `helion run dbshell ...`). If run
from a directory containing the *stackato.yml* file, the application and
service names may be omitted.

#### Using helion tunnel[](#id3 "Permalink to this headline")

**Note**

This method of database import is available for compatibility with Cloud
Foundry. It tends to be slower than using `helion run ...`{.docutils
.literal}.

To import data from a `mysqldump` into an existing
MySQL database service, use the `tunnel` command:

    $ helion tunnel <servicename>

    Password: ********
    Getting tunnel url: OK, at https://tunnel-xxxxx.helion-xxxx.local
    Getting tunnel connection info: OK

    Service connection info:
    +----------+-----------------------------------+
    | Key      | Value                             |
    +----------+-----------------------------------+
    | username | uT9efVVFCk                        |
    | password | pHFitpIU1z                        |
    | name     | d5eb2468f70ef4997b1514da1972      |
    +----------+-----------------------------------+

    1. none
    2. mysql
    3. mysqldump
    Which client would you like to start?

Choose option **1. none** which will allow for command line access to
the database. A MySQL service is configured on Port 10000, so open a new
Terminal window to enter commands with.

Then, import an SQL file with the following command:

    $ mysql --protocol=TCP --host=localhost --port=10000 --user=<user> --password=<pass> <name> < mydatabase.sql

See the [*tunnel*](#database-tunnel) command documentation for other
ways of accessing a MySQL database. See [*Backing up a MySQL
database*](#bestpractices-backing-up-mysql) for details on how to create
a `mysqldump` backup that can then be imported into
another database service.

### Database Version Changes[](#database-version-changes "Permalink to this headline")

The VCAP\_SERVICES environment variable in Application Lifecycle Service does not include
version numbers in the service name string. This can cause problems when
migrating applications from Cloud Foundry v1 systems which reference
versioned database names in VCAP\_SERVICES.

There are two application level fixes for this issue:

#### Method 1[](#method-1 "Permalink to this headline")

Update references to VCAP\_SERVICES in the application code to exclude
version numbers. For example:

    MySQL:         'mysql-5.x' -> 'mysql'
    PostgreSQL:    'postgresql-x.x' -> 'postgresql'
    Redis:         'redis-2.x' -> 'redis'

#### Method 2[](#method-2 "Permalink to this headline")

Update the application code to use the DATABASE\_URL environment
variable. See [*Using Database Services*](#database-accessing) for
general information and the following language-specific documentation:

-   [*Perl Data
    Services*](/als/v1/user/deploy/languages/perl/#perl-data-services)
-   [*PHP Data
    Services*](/als/v1/user/deploy/languages/php/#php-data-services)
-   [*Python Data
    Services*](/als/v1/user/deploy/languages/python/#python-data-services)

The following changes to sample applications show this modification:

-   PERL:
    <https://github.com/Stackato-Apps/bugzilla/commit/414804f3c02dab5104f048c013b8a3127e5268b2>
-   PYTHON:
    <https://github.com/Stackato-Apps/django-gtd/commit/fdc7361086c5a1f9d2b10ee5e7af918e9f60b999>
-   PHP:
    <https://github.com/Stackato-Apps/owncloud-core/commit/3bd87948f48910f27fa1e059e863bcf312cce5f3>

SQLite[](#sqlite "Permalink to this headline")
-----------------------------------------------

Applications can use an [SQLite database](http://www.sqlite.org/) as an
alternative to Application Lifecycle Service database services. However, as the filesystem of
an application container is ephemeral (i.e. it is destroyed when an
application is stopped, restarted, or updated), you should always store
the SQLite file on a [*Persistent File
System*](/als/v1/user/services/filesystem/#persistent-file-system) mount point to avoid
losing data.

### [Table Of Contents](/als/v1/index-2/)

-   [Data Services](#)
    -   [Intro](#intro)
    -   [Configuring Application Lifecycle Service Data
        Services](#configuring-helion-data-services)
        -   [Using stackato.yml](#using-stackato-yml)
        -   [Using helion push](#using-helion-push)
        -   [Creating and Binding
            Services](#creating-and-binding-services)
    -   [Using Database Services](#using-database-services)
        -   [DATABASE\_URL](#database-url)
        -   [Database-Specific URLs](#database-specific-urls)
        -   [STACKATO\_SERVICES](#helion-services)
        -   [VCAP\_SERVICES](#vcap-services)
    -   [Using External Databases](#using-external-databases)
    -   [Hard-coded Database
        Connections](#hard-coded-database-connections)
    -   [Accessing Database Services](#accessing-database-services)
        -   [Using dbshell](#using-dbshell)
        -   [Using Tunnel](#using-tunnel)
        -   [Pre-populating a database while pushing an
            app](#pre-populating-a-database-while-pushing-an-app)
        -   [Backing up a MySQL database](#backing-up-a-mysql-database)
            -   [Using helion run](#using-helion-run)
            -   [Using helion tunnel](#using-helion-tunnel)
        -   [Importing a MySQL database](#importing-a-mysql-database)
            -   [Using helion run](#id2)
            -   [Using helion tunnel](#id3)
        -   [Database Version Changes](#database-version-changes)
            -   [Method 1](#method-1)
            -   [Method 2](#method-2)
    -   [SQLite](#sqlite)
