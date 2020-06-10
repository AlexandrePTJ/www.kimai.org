---
title: Configurations
description: Kimai configurations files and basic setup, local overwrites and the cache
toc: true
---

This is an introduction into the configuration options and files, which are used by Kimai and an explanation on how to change them. 
 
Specific configuration settings are explained in the respective documentation chapters.

## Environment specific settings (.env)

The basic settings, which are required for Kimai to work are stored in the `.env` file:
 
- `MAILER_URL` - smtp connection for emails
- `MAILER_FROM` - application specific "from" address for all emails
- `APP_ENV` - environment for the runtime (use `prod` if you are unsure)
- `DATABASE_URL` - database connection for storing all application data
- `APP_SECRET` - secret used to encrypt session cookies (users will be logged out if you change it) 

## Config files

Configuration of Kimai is done through the files in the `config/` directory, the most important ones are:

- `.env` - your environment and connection settings
- `config/packages/kimai.yaml` - Kimai settings
- `config/packages/fos_user.yaml` - user management
- `config/packages/local.yaml` - **additional Kimai configurations for your needs (does not exist by default - see below!)**

There are several other configurations that could potentially be interesting for you in [config/packages/*.yaml]({{ site.kimai_v2_file }}/config/packages/).

If you want to adjust a setting from any of these files, apply them through the use of your own `local.yaml` (see below).

## Changing configurations

You should NOT edit any of the configuration files (eg. `config/packages/kimai.yaml`) directly, as they contains default settings and will be overwritten during an update.

Instead create the file `config/packages/local.yaml` and save your own settings in there. 

### local.yaml

This file will NEVER be shipped with Kimai, you have to create it before you change settings the first time (eg. `touch config/packages/local.yaml`).
Having your custom settings in `local.yaml` allows you to easily update Kimai. This is the same concept which is used for the `.env` file.

An example `config/packages/local.yaml` file might look like this:

```yaml
kimai:
    timesheet:
        rounding:
            default:
                begin: 15
                end: 15

admin_lte:
    options:
        default_avatar: build/apple-touch-icon.png
```

The `local.yaml` file will be imported as last configuration file, so you can overwrite any setting from the `config/packages/` directory.

Whenever the documentation asks you to edit a yaml file from the `config/packages/` directory, it means you should copy 
this specific configuration key to your `local.yaml` in order to overwrite the default configuration.

{% include alert.html icon="fas fa-exclamation" type="warning" alert="Be consistent with the indentation and don't mix spaces and tabs, YAML is very sensitive about that!" %}

### Reload changed configurations

When you change your `local.yaml` configuration file, Kimai will not see this change immediately. 
You have to reload the configurations by rebuilding the cache.

{% include cache-refresh.html %} 

It might be necessary to execute these commands as webserver user, 
read the [Installation docs]({% link _documentation/installation.md %}) for more details.

Depending on your setup and the way you call the cache command, you have to fix directory permissions afterwards. 
 
{% include file-permissions.html %} 

## System-configuration screen

You can edit most of the configurations from the Kimai UI directly.

This screen is only visible to users with the permission `system_configuration` which is by default given to `ROLE_SUPER_ADMIN`.

Each setting in this screen is also available in the config file (`config/packages/kimai.yaml`) where you might find 
additional information or links to the correct documentation chapter.

## User preferences vs. system settings

A user has several preferences, which change the behaviour how he interacts with Kimai.

Check out the [user preferences documentation]({% link _documentation/user-preferences.md %}) to find out more.

## Adding system configuration

As plugin developer you can add your own sections to the system configuration screen, see [developer documentation]({% link _documentation/developers.md %}).
