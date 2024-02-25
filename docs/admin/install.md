# Installing the App in Nautobot

Here you will find detailed instructions on how to **install** and **configure** the App within your Nautobot environment.

## Prerequisites

- The app is compatible with Nautobot 2.0.0 and higher.
- Databases supported: PostgreSQL, MySQL
- [Yelp API Key](https://docs.developer.yelp.com/docs/fusion-authentication)

!!! note
    Please check the [dedicated page](compatibility_matrix.md) for a full compatibility matrix and the deprecation policy.

## Install Guide

!!! note
    Apps can be installed from the [Python Package Index](https://pypi.org/) or locally. See the [Nautobot documentation](https://docs.nautobot.com/projects/core/en/stable/user-guide/administration/installation/app-install/) for more details. The pip package name for this app is [`nautobot_lunch`](https://pypi.org/project/nautobot_lunch/).

The app is available as a Python package via PyPI and can be installed with `pip`:

```shell
pip install nautobot_lunch
```

To ensure Nautobot nautobot_lunch is automatically re-installed during future upgrades, create a file named `local_requirements.txt` (if not already existing) in the Nautobot root directory (alongside `requirements.txt`) and list the `nautobot_lunch` package:

```shell
echo nautobot_lunch >> local_requirements.txt
```

Once installed, the app needs to be enabled in your Nautobot configuration. The following block of code below shows the additional configuration required to be added to your `nautobot_config.py` file:

- Append `"nautobot_nautobot_lunch"` to the `PLUGINS` list.
- Append the `"nautobot_nautobot_lunch"` dictionary to the `PLUGINS_CONFIG` dictionary and override any defaults.

```python
# In your nautobot_config.py
PLUGINS = ["nautobot_nautobot_lunch"]

# PLUGINS_CONFIG = {
#   "nautobot_nautobot_lunch": {
#     ADD YOUR SETTINGS HERE
#   }
# }
```

Once the Nautobot configuration is updated, run the Post Upgrade command (`nautobot-server post_upgrade`) to run migrations and clear any cache:

```shell
nautobot-server post_upgrade
```

Then restart (if necessary) the Nautobot services which may include:

- Nautobot
- Nautobot Workers
- Nautobot Scheduler

```shell
sudo systemctl restart nautobot nautobot-worker nautobot-scheduler
```

## App Configuration

The app behavior can be controlled with the following list of settings:

| Key                 | Environment Variable      | Example  | Default | Description                              |
|---------------------|---------------------------|----------|---------|------------------------------------------|
| `yelp_api_key`      | YELP_API_KEY              | `Qq----` | ``      | Set the Yelp API Key                     |
| `platform_slug_map` | NAUTOBOT_LUNCH_CACHE_TIME | `3600`   | `3600`  | How long to cache the queries in seconds |
