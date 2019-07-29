<h2 align="center">Instagram Private API</h2>
<p align="center">
    <a href="https://travis-ci.org/breuerfelix/instapi"><img alt="Build Status" src="https://travis-ci.org/breuerfelix/instapi.svg?branch=master"></a>
    <a href="https://instapi.readthedocs.io/en/latest"><img alt="Documentation Status" src="https://readthedocs.org/projects/instapi/badge/?version=latest"></a>
    <a href="https://github.com/breuerfelix/instapi/blob/master/LICENSE"><img src="https://img.shields.io/github/license/breuerfelix/instapi.svg" /></a>
    <a href="https://pypi.org/project/instapi/"><img alt="PyPI" src="https://img.shields.io/pypi/v/instapi"></a>
    <a href="https://github.com/breuerfelix/instapi"><img alt="Code style: black" src="https://img.shields.io/badge/code%20style-black-000000.svg"></a>
</p>

A Python wrapper for the Instagram private API with no 3rd party dependencies.

## Overview

I wrote this to access Instagram's API when they clamped down on developer access. Because this is meant to achieve [parity](COMPAT.md) with the [official public API](https://www.instagram.com/developer/endpoints/), methods not available in the public API will generally have lower priority.

Problems? Please check the [docs](https://instapi.readthedocs.io/en/latest/) before submitting an issue.

## Features

- Supports many functions that are only available through the official app, such as:
  - Multiple feeds, such as [user feed](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.user_feed), [location feed](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.feed_location), [tag feed](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.feed_tag), [popular feed](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.feed_popular)
  - Post a [photo](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.post_photo) or [video](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.post_video) to your feed or stories
  - [Like](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.post_like)/[unlike](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.delete_like) posts
  - Get [post comments](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.media_comments)
  - [Post](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.post_comment)/[delete](https://instapi.readthedocs.io/en/latest/api.html#instagram_web_api.Client.delete_comment) comments
    https://instapi.readthedocs.io/en/latest/api/#instapi.Client.comment_unlike
  - [Like](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.comment_like)/[unlike](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.comment_unlike) comments
  - [Follow](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.friendships_create)/[unfollow](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.friendships_destroy) users
  - User [stories](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client.user_story_feed)
  - And [more](https://instapi.readthedocs.io/en/latest/api.html#instapi.Client)!
- Compatible with functions available through the public API using the [ClientCompatPatch](https://instapi.readthedocs.io/en/latest/api.html#instapi.ClientCompatPatch) utility class

## Documentation

Documentation is available at [instapi.rtfd.io](https://instapi.readthedocs.io/en/latest/).

## Install

Install with pip:

``pip install instapi``

To update:

``pip install instapi --upgrade``

To update with latest repo code:

``pip install instapi --upgrade --force-reinstall``

Tested on Python 3.

## Usage

The [app API client](instapi/) emulates the official app and has a larger set of functions.

The [``examples/``](examples/) and [``tests/``](tests/) are a good source of detailed sample code on how to use the clients, including a simple way to save the auth cookie for reuse.

### Use the [official app's API](instapi/)

```python

from instapi import Client, ClientCompatPatch

user_name = 'YOUR_LOGIN_USER_NAME'
password = 'YOUR_PASSWORD'

api = Client(user_name, password)
results = api.feed_timeline()
items = [item for item in results.get('feed_items', [])
         if item.get('media_or_ad')]
for item in items:
    # Manually patch the entity to match the public api as closely as possible, optional
    # To automatically patch entities, initialise the Client with auto_patch=True
    ClientCompatPatch.media(item['media_or_ad'])
    print(item['media_or_ad']['code'])
```

### Avoiding Re-login

You are advised to persist/cache the auth cookie details to avoid logging in every time you make an api call. Excessive logins is a surefire way to get your account flagged for removal. It's also advisable to cache the client details such as user agent, etc together with the auth details.

The saved auth cookie can be reused for up to **90 days**.

## Support

Make sure to review the [contributing documentation](CONTRIBUTING.md) before submitting an issue report or pull request.

## Legal

Disclaimer: This is not affliated, endorsed or certified by Instagram. This is an independent and unofficial API. Strictly **not for spam**. Use at your own risk.
