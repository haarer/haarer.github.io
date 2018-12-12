---
layout: post
title: "Forking Ardublockly"
categories: Arduino,Blockly
---

[Ardublockly][1] is a great project - it brings blockly to the Arduino platforms.

Unfortunatly it is not translated to German.  In order to make it available to my son, i wanted to add a translation to it. The excellent Documentation of the project makes this quite easy.

**Adding a Language**

How to add a new language is [documented here][2]


It is necessary to edit/create the following files

    
* *ardublockly/ardublockly_lang.js* (added the language to the list of languages)
* *ardublockly/msg/de.js* (copied from the english file and translated it)
* *blockly/msg/json/de.json* (not changed)
* *blockly/msg/json/de_ardublockly.json* (copied from the english file and translated it)

**Build Prerequisites**

For building Ardublockly from the source the following is needed:
* Python 2.7
* Python 3
  * python module pyinstaller
  * python module MKDocs
* Node.js
  
Note that the python modules are installed into the Python 3 Environment.
 
**Building from Source**

* cd blockly
* <Python27 path>\python.exe build.py
* cd ..
* python package/build_pyinstaller.py
* cd package\electron
* npm install
* npm run release
* cd.. cd ..
* python package\build_docs.py

**Rebuilding Ardublockly Translation**

In order to test the new translation, the app has to be repacked. A complete rebuild is not necessary. This is done by a python script.

* cd blockly
* <Python27 path>\python.exe build.py

The script must be called in a python 2.7 environment.

After the script has run, it is necessary to clean the chromium cache - if this step is omitted, the changes are not visible.

Just remove all files
from *arduexec/appdata/Cache*.



  [1]: https://ardublockly.embeddedlog.com/
  [2]: https://github.com/carlosperate/ardublockly/wiki/Add-New-Language 
