<!-- source: https://wiki.gentoo.org/wiki/Okupy/Profiling_and_Debugging | group: Gentoo Wiki (Main) | wiki-title: Okupy/Profiling and Debugging -->
---
title: Okupy/Profiling and Debugging
url: https://wiki.gentoo.org/wiki/Okupy/Profiling_and_Debugging
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: "62605b9a6c156350"
license: CC BY-SA 4.0
---

# Okupy/Profiling and Debugging

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Profiling

Required packages: hotshot

### Option 1

- Create a new file and name it 'middleware.py'.
- Add the following code: [https://gist.github.com/Miserlou/3649773/raw/72e73609e67f70faaa31c73f25911ba15e384d54/middleware.py](https://gist.github.com/Miserlou/3649773/raw/72e73609e67f70faaa31c73f25911ba15e384d54/middleware.py)
- In the MIDDLEWARE\_CLASSES of your local.py/development.py, include the line. 'okupy.accounts.middleware.ProfileMiddleware',
- To use the profiler, simply add the string '?prof' to the end of your URL. e.g. [http://127.0.0.1:8000/devlist/?prof](http://127.0.0.1:8000/devlist/?prof)
- Reference: [https://gun.io/blog/fast-as-fuck-django-part-1-using-a-profiler/](https://gun.io/blog/fast-as-fuck-django-part-1-using-a-profiler/)

### Option 2

- Follow this guide: [https://code.djangoproject.com/wiki/ProfilingDjango](https://code.djangoproject.com/wiki/ProfilingDjango)

## Debugging

- Required packages: django-debug-toolbar
- Optional packages: [https://github.com/django-debug-toolbar/django-debug-toolbar/wiki/3rd-Party-Panels](https://github.com/django-debug-toolbar/django-debug-toolbar/wiki/3rd-Party-Panels)
- Make sure your IP is listed in the INTERNAL\_IPS local.py/development.py settings. If you are working locally this will be: INTERNAL\_IPS = ('127.0.0.1',)
- Add the following middleware to local.py/development.py file: FILE**`local.py or development.py`** MIDDLEWARE\_CLASSES = ( 'debug\_toolbar.middleware.DebugToolbarMiddleware', )
- Add debug\_toolbar to your INSTALLED\_APPS setting INSTALLED\_APPS = ( 'debug\_toolbar', )
- Add a tuple called DEBUG\_TOOLBAR\_PANELS to your settings.py file that specifies the full Python path to the panel that you want included in the Toolbar. FILE**`settings.py`** DEBUG\_TOOLBAR\_PANELS = ( 'debug\_toolbar.panels.version.VersionDebugPanel', 'debug\_toolbar.panels.timer.TimerDebugPanel', 'debug\_toolbar.panels.settings\_vars.SettingsVarsDebugPanel', 'debug\_toolbar.panels.headers.HeaderDebugPanel', 'debug\_toolbar.panels.request\_vars.RequestVarsDebugPanel', 'debug\_toolbar.panels.template.TemplateDebugPanel', 'debug\_toolbar.panels.sql.SQLDebugPanel', 'debug\_toolbar.panels.signals.SignalDebugPanel', 'debug\_toolbar.panels.logger.LoggingPanel', )

Reference: [https://github.com/django-debug-toolbar/django-debug-toolbar](https://github.com/django-debug-toolbar/django-debug-toolbar)

## Front-end profiling

- Firefox: Shift+F5 to start profiler or Right-click --> Inspect element --> Profiler

  - Features: 3D view, Responsive design mode, times

- TODO: Chrome Dev tools
