---
layout: default-devplatform
permalink: /als/v1/user/quick-start/
---

Quick Start[](#index-0 "Permalink to this headline")
=====================================================

This guide covers the basics of using an Application Lifecycle Service PaaS. To follow along,
you will need:

-   a user account
-   a copy of the `helion` CLI client
-   a web browser

If you want to run your own Application Lifecycle Service "micro cloud" system for local
development and testing,  download a copy of the Application Lifecycle Service
VM  and follow the
setup instructions in the Admin Guide.

Management Console[](#management-console "Permalink to this headline")
-----------------------------------------------------------------------

The web interface for Application Lifecycle Service is called the [*Management
Console*](/als/v1/admin/console/customize/#management-console). You can use it to:

-   see your current usage and quota
-   monitor and manage applications you have deployed
-   see which runtimes, frameworks, and services are available
-   deploy applications from the App Store

To use it, open the API Endpoint URL (e.g. *https://api.stacka.to*) in
your favorite browser. Log in with the username and password that have
been set up for you on the system.

The interface should be mostly self-explanatory. It exposes most of the
functionality you have access to as an end user, but to deploy your own
applications from source code on your local machine you will need to use
the `helion` CLI client.

Application Lifecycle Service Client[](#helion-client "Permalink to this headline")
-----------------------------------------------------------------

The `helion` client is used for pushing
applications to Application Lifecycle Service and interacting with the system from the
command line.

1.  Download the client for your platform (Windows, OS X, Linux x86,
    Linux x64)
2.  Unzip the archive in a convenient directory.
3.  Add the the executable to your system/shell \$PATH by:

> -   moving it to a directory in your \$PATH,
> -   creating a symlink from a directory in your \$PATH, or
> -   creating a shell alias for the executable.

4.  Confirm that the client is installed correctly by running
    `helion help`.

The [*Application Lifecycle Service Client Command
Reference*](/als/v1/user/reference/client-ref/#command-ref-client) has a full
list of commands and options.

### Target and Login[](#target-and-login "Permalink to this headline")

To connect the `helion` client to the PaaS, use
the `target` command to specify the API Endpoint
URL. This is normally the hostname prepended with "api." for example:

``` {.literal-block}
$ helion target api.example.hphelion.com
Target:       https://api.example.hphelion.com
Organization: <none>
Space:        <none>
Successfully targeted to [https://api.example.hphelion.com]
```

Once you have successfully targeted Application Lifecycle Service, you must authenticate
using `helion login` with the credentials that
have been created for you:

``` {.literal-block}
$ helion login user@example.com
Attempting login to [https://api.example.hphelion.com]
Password: ********
Successfully logged into [https://api.example.hphelion.com]
...
```

Setting Organization and Space[](#setting-organization-and-space "Permalink to this headline")
-----------------------------------------------------------------------------------------------

If you are logging in for the first time, your user account will not
automatically choose an [*Organization and
Space*](/als/v1/user/deploy/orgs-spaces/#orgs-spaces) for deployment.

To set the Organization you belong to:

    $ helion switch-org *org-name*
    Switching to organization *org-name* ... OK
    Unsetting current space ... OK
    Target:       https://api.stacka.to
    Organization: *org-name*
    Space:        <none>

To set the Space you want to deploy applications to:

    $ helion switch-space dev
    Switching to organization *org-name* ... OK
    Switching to space dev ... OK
    Target:       https://api.stacka.to
    Organization: test-org
    Space:        dev

**Note**

If your account is not a member of a Space, you will need an
Organization Manager to add you as a Developer. If you are an
Organization manager (or Admin), you can create create new Spaces in the
[*Management Console*](/als/v1/admin/console/customize/#user-console-space) or with
the [*helion
create-space*](/als/v1/user/reference/client-ref/#command-create-space)
command.

Deploying Apps[](#deploying-apps "Permalink to this headline")
---------------------------------------------------------------

The steps for deploying applications will vary slightly depending on the
language or framework used, but the basic command is:

``` {.literal-block}
$ helion push --as [appname]
```

By default, the client will take application code from your current
working directory and push it to Application Lifecycle Service for further staging and
configuration, prompting for additional configuration information as
necessary.

### Sample Applications[](#sample-applications "Permalink to this headline")

Numerous sample applications are available from
[Application Lifecycle Service-Apps](https://github.com/stackato-Apps) on Github, each with a
README.md file showing how to deploy it on Application Lifecycle Service.

Many of these have a
[*stackato.yml*](/als/v1/user/deploy/stackatoyml/#stackato-yml) or
[*manifest.yml*](/als/v1/user/deploy/manifestyml/#manifest-yml) configuration
files, so you can skip the interactive prompts by using the
`-n` option:

``` {.literal-block}
$ helion push -n [appname]
```

### Test the Application[](#test-the-application "Permalink to this headline")

The output of `helion push` will show the URL of
the running application. Paste this URL into a web browser to try the
application or run:

``` {.literal-block}
$ helion open [appname]
```

Documentation on configuring applications in specific languages and
frameworks is available in the [*Deploying
Apps*](/als/v1/user/deploy/#deploying-apps) section.

### [Table Of Contents](/als/v1/index-2/)

-   [Quick Start](#)
    -   [Management Console](#management-console)
    -   [Application Lifecycle Service Client](#helion-client)
        -   [Target and Login](#target-and-login)
    -   [Setting Organization and
        Space](#setting-organization-and-space)
    -   [Deploying Apps](#deploying-apps)
        -   [Sample Applications](#sample-applications)
        -   [Test the Application](#test-the-application)
