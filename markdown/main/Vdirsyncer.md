<!-- source: https://wiki.gentoo.org/wiki/Vdirsyncer | group: Gentoo Wiki (Main) | wiki-title: Vdirsyncer -->
---
title: Vdirsyncer
url: https://wiki.gentoo.org/wiki/Vdirsyncer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-29"
fingerprint: "4c5fb84f01a19fbe"
license: CC BY-SA 4.0
---

# Vdirsyncer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**vdirsyncer** is a command-line tool for synchronizing calendars and address books between a variety of servers and the local filesystem.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-python/vdirsyncer`
### Additional software

## Configuration

### Files

- \~/.config/vdirsyncer/config - Local (per user) configuration file.

## Usage

### Invocation

`user $``vdirsyncer --help`
(Paste command help output here.)

\</pre>
## Caveats

## Tips

## Troubleshooting

### Google calendars and contacts

vdirsyncer supports synchronization with Google calendars, but requires some additional steps to get working.

You need to register vdirsyncer as an application yourself to obtain client\_id and client\_secret. It is against Google’s Terms of Service to hardcode those into open source software. The steps are as follows:

- Go to the [Google API manager](https://console.cloud.google.com/apis/dashboard).
- Login in as yourself.
- Create a new project with any name (e.g. vdirsyncer). You do this via the toolbar at top of page.
- Click on "Enable APIs and Services".

- Enter CalDAV (for calendars) in the search box (repeat all follow steps for CardDAV if you wish to sync contacts as well) .
- Click on CalDAV API, enable the API.
- Maybe you need to Click Manage (depends if you are returning), a menu tree/sidebar should appear on left.

- Select Oauth Consent screen.
- Fill out the Consent screen however you like.
- There are 3 screens to fill out, on the last it's a good idea to add yourself as a test user. If you don't the authorisation may fail.

- Next select Credentials in the sidebar.
- Click add Credentials.
- Select OAuth Client ID.
- Select Web application.
- Under "Authorised redirect URIs" add [http://127.0.0.1:8088](http://127.0.0.1:8088)
- Press "Create".
- Note down the client id and client secret.

- In your vdirsyncer config file; paste in the client ID and secret.

**`~/.config/vdirsyncer/config`**

```
...
[storage example_for_google_calendar]
type = "google_calendar"
client_id = "..."
client_secret = "..."
...
```


- Now you need to edit the port number used in the google.py file found at /usr/lib/python3.10/site-packages/vdirsyncer/storage. The redirect URL needs to use port 8088.

- Find the three lines

**`/usr/lib/python3.10/site-packages/vdirsyncer/storage/google.py`**

```
...
local_server = wsgiref.simple_server.make_server(                                                                        
    host, 0, wsgi_app, handler_class=_WSGIRequestHandler                                                              
) 
...
```
- Change '0' to '8088'.
- Next run vdirsyncer discover (assuming you have a fully formed config file as per vdirsyncer documenation).

`user $````
vdirsyncer discover
```
- This should open a link in your browser to authorise vdirsyncer.
- Proceed with Google authentication until fully complete and you can go no further.

- Switch back to command line where vdirsyncer was waiting, it should now have moved on to the next step in the discovery process.

### Unmerge

`root #``emerge --ask --depclean --verbose dev-python/vdirsyncer`
