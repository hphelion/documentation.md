---
layout: default-devplatform
permalink: /als/v1/user/deploy/orgs-spaces/
---

Organizations & Spaces[](#organizations-spaces "Permalink to this headline")
=============================================================================

Organizations and Spaces are the main organizational units in Application Lifecycle Service.

-   Organizations have Users, Spaces, and Domains
-   Spaces have Users, Applications, and Service Instances
-   Applications have Routes (which are derived from Domains)

Organizations[](#organizations "Permalink to this headline")
-------------------------------------------------------------

An organization is a top-level group of users, spaces, and domains. Only
Application Lifecycle Service admins (accounts with global superuser privileges) can manage
Organizations.

Each organization is assigned a [*Quota
Definition*](/als/v1/admin/server/configuration/#server-config-quota-definitions),
a set of limits on memory, applications, and service instances which is
share between all members of the organization.

Spaces[](#spaces "Permalink to this headline")
-----------------------------------------------

An organization can contain multiple spaces (e.g. **development**,
**test**, and **production**). A domain can be mapped to multiple spaces
but a route can be mapped to only one space.

Domains[](#domains "Permalink to this headline")
-------------------------------------------------

A domain in Application Lifecycle Service is a fully-qualified, second-level or lower domain
name (e.g. "example.com" or "helion.example.com").

Organizations and spaces can have custom domains, but are often able to
use a system domain by default as well (e.g. "myorg.net" and
"helion.example.com"). Domains belong to an organization. They are
associated with one or more spaces within that organization, but are not
directly bound to apps. Apps are assigned a "hostname + domain"
combination called a Route.

Routes[](#routes "Permalink to this headline")
-----------------------------------------------

A route is a virtual hostname followed by a domain name or
fully-qualified sub-domain (e.g. "myapp.myorg.example.com").

Management[](#management "Permalink to this headline")
-------------------------------------------------------

You can manage spaces and organizations with the [*helion
client*](/als/v1/user/client/#client) or the [*Management
Console*](/als/v1/admin/console/customize/#user-console-organizations).

Users & Roles[](#users-roles "Permalink to this headline")
-----------------------------------------------------------

Application Lifecycle Service users can take on different roles within Orgs and Spaces. These
roles can be assigned by a Manager of the relevant scope or an Application Lifecycle Service
Admin:

### Org Roles[](#org-roles "Permalink to this headline")

-   Manager: Can invite/manage users, select/change the plan, establish
    spending limits
-   Billing Manager: Can edit/change the billing account info, payment
    info
-   Auditor: View only access to all org and space info, settings,
    reports

### Space Roles[](#space-roles "Permalink to this headline")

-   Space Manager: Can invite/manage users, enable features for a given
    space
-   Space Developer: Can create, delete, manage applications and
    services, full access to all usage reports and logs
-   Space Auditor: View only access to all space information, settings,
    reports, logs

### [Table Of Contents](/als/v1/index-2/)

-   [Organizations & Spaces](#)
    -   [Organizations](#organizations)
    -   [Spaces](#spaces)
    -   [Domains](#domains)
    -   [Routes](#routes)
    -   [Management](#management)
    -   [Users & Roles](#users-roles)
        -   [Org Roles](#org-roles)
        -   [Space Roles](#space-roles)
