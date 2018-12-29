---
layout: post
title: "Forking Ardublockly"
categories: Arduino,Blockly
---

[Ardublockly][1] is a great project - it brings blockly to the Arduino platforms.

Unfortunatly it is not translated to German.  In order to make it available to my son, i wanted to add a translation to it. The excellent Documentation of the project makes this quite easy. [The fork is here][3]

I have also added blocks to control WS2812 color LED strips

![This is the result so far](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-12-12-Ardublockly.png)
![The LED Strip](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-12-12-Ledstrip.jpg)

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

**Obtaining the Source**

```
git clone https://github.com/haarer/ardublockly.git
cd ardublockly
git submodule update --init --recursive
```
 
**Building from Source**

First build the language files

```
cd blockly
\<Python27 path\>\python.exe build.py
cd ..
```

Second, build the python server
```
python package/build_pyinstaller.py
```

Third, build the desktop application

```
cd package\electron
npm install
npm run release
cd..
cd ..
python package\build_docs.py
```

**Testing**

You can start the python server

```python start.py```

Or you can start the desktop app

```arduexec\ardublockly```
  
  
**Rebuilding Ardublockly Translation**

In order to test the new translation, just the language files have to  be rebuilt. A complete rebuild is not necessary. This is done by a python script.

* cd blockly
* \<Python27 path\>\python.exe build.py

The script must be called in a python 2.7 environment.

If you want to test the changed translation by the desktop app, it is necessary to clean the chromium cache - if this step is omitted, the changes are not visible.

Just remove all files from *arduexec/appdata/Cache*.


  [1]: https://ardublockly.embeddedlog.com/
  [2]: https://github.com/carlosperate/ardublockly/wiki/Add-New-Language 
  [3]: https://github.com/haarer/ardublockly
