# Maloja

[![](https://img.shields.io/pypi/v/malojaserver?style=for-the-badge)](https://pypi.org/project/malojaserver/)
[![](https://img.shields.io/pypi/dm/malojaserver?style=for-the-badge)](https://pypi.org/project/malojaserver/)
[![](https://img.shields.io/github/stars/krateng/maloja?style=for-the-badge&color=purple)](https://github.com/krateng/maloja/stargazers)
[![](https://img.shields.io/pypi/l/malojaserver?style=for-the-badge)](https://github.com/krateng/maloja/blob/master/LICENSE)

Simple self-hosted music scrobble database to create personal listening statistics. No recommendations, no social network, no nonsense.

You can check [my own Maloja page](https://maloja.krateng.ch) to see what it looks like.


> **IMPORTANT**: With the update 2.0, Maloja has been refactored into a Python package and the old update script no longer works. If you're still on version 1, see [below](#update).

## Table of Contents
* [Why not Last.fm / Libre.fm / GNU FM?](#why-not-lastfm--librefm--gnu-fm)
* [How to install](#how-to-install)
	* [New Installation](#new-installation)
	* [Update](#update)
* [How to use](#how-to-use)
* [How to scrobble](#how-to-scrobble)
	* [Native API](#native-api)
	* [Standard-compliant API](#standard-compliant-api)
	* [Manual](#manual)

## Why not Last.fm / Libre.fm / GNU FM?

Maloja is **self-hosted**. You will always be able to access your data in an easily-parseable format. Your library is not synced with any public or official music database, so you can **follow your own tagging schema** or even **group associated artists together** in your charts.

Maloja also gets **rid of all the extra stuff**: social networking, radios, recommendations, etc. It only keeps track of your listening history and lets you analyze it.

Maloja's database has one big advantage: It supports **multiple artists per track**. This means artists who are often just "featuring" in the track title get a place in your charts, and **collaborations between several artists finally get credited to all participants**. This allows you to get an actual idea of your artist preferences over time.

Also neat: You can use your **custom artist or track images**.

## How to install

### New Installation

1) Make sure you have Python 3.5 or higher installed. You also need some basic packages that should be present on most systems, but I've provided simple shell scripts for Alpine and Ubuntu to get everything you need.

2) If you'd like to display images, you will need API keys for [Last.fm](https://www.last.fm/api/account/create) and [Fanart.tv](https://fanart.tv/get-an-api-key/) (you need a project key, not a personal one). These are free of charge!

3) Download Maloja with the command `pip install malojaserver`. Make sure to use the correct python version (Use `pip3` if necessary).

4) (Recommended) Put your server behind a reverse proxy for SSL encryption.

### Update

* If you use a version before 2.0 (1.x), install the package as described above, then manually copy all your user data to your `~/.local/share/maloja` folder.
* Otherwise, simply run the command `maloja update` or use `pip`s update mechanic.

## How to use

1) Start and stop the server with

		maloja start
		maloja stop
		maloja restart


2) If you would like to import all your previous last.fm scrobbles, use [benfoxall's website](https://benjaminbenben.com/lastfm-to-csv/) ([GitHub page](https://github.com/benfoxall/lastfm-to-csv)). Use the command

		maloja import *filename*

	to import the downloaded file into Maloja.

3) Various folders have `.info` files with more information on how to use their associated features.

4) You can also have a look at the `settings.ini` file.

5) If you'd like to implement anything on top of Maloja, visit `/api_explorer`.

6) To backup your data, run

		maloja backup

	or, to only backup essential data (no artwork etc)

		maloja backup -l minimal

## How to scrobble

### Native API

If you use Plex Web, Spotify, Bandcamp, Soundcloud or Youtube Music on Chromium, you can use the included extension (also available on the [Chrome Web Store](https://chrome.google.com/webstore/detail/maloja-scrobbler/cfnbifdmgbnaalphodcbandoopgbfeeh)). Make sure to enter the random key Maloja generates on first startup in the extension settings.

If you want to implement your own method of scrobbling, it's very simple: You only need one POST request to `/api/newscrobble` with the keys `artist`, `title` and `key` - either as form-data or json.

### Standard-compliant API

You can use any third-party scrobbler that supports the audioscrobbler (GNUFM) or the ListenBrainz protocol. This is still very experimental, but give it a try with these settings:

GNU FM | &nbsp;
------ | ---------
Gnukebox URL | Your Maloja URL followed by `/api/s/audioscrobbler`
Username | Any name, doesn't matter
Password | Any of your API keys

ListenBrainz | &nbsp;
------ | ---------
API URL | Your Maloja URL followed by `/api/s/listenbrainz`
Username | Any name, doesn't matter
Auth Token | Any of your API keys

These are tested with the Pano Scrobbler and the Simple Last.fm Scrobbler for Android. I'm thankful for any feedback whether other scrobblers work!

It is recommended to define a different API key for every scrobbler you use in `clients/authenticated_machines.tsv` in your Maloja folder.

### Manual

If you can't automatically scrobble your music, you can always do it manually on the `/manual` page of your Maloja server.
