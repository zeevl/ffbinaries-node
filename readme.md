# ffbinaries downloader

[![NPM Version][npm-img]][npm-url]
[![NPM Downloads][npm-dl-img]][npm-url]

[npm-url]: https://npmjs.org/package/ffbinaries
[npm-img]: https://img.shields.io/npm/v/ffbinaries.svg
[npm-dl-img]: https://img.shields.io/npm/dm/ffbinaries.svg


Downloads precompiled ffmpeg, ffprobe, ffplay and ffserver binaries from http://ffbinaries.com.

This module is meant to be used programatically, i.e. as a build step or in a postinstall script.


# Project status

**This project can be considered stable now since it reached version 0.1.0. That's the minimum version you should be running.**

The module downloads, caches and then extracts the binaries from archives correctly. All the features can be expected to work at this point.

If you're experiencing issues please update to the newest version and run `ffbinaries clearcache`.

Please raise issues and pull requests for features you'd like to see on GitHub: https://github.com/vot/ffbinaries-node.


## ffbinaries.com API

http://ffbinaries.com service provides a versioned API. This will prevent issues with data structure changes in the future.

The API service is running this application: https://github.com/vot/ffbinaries-api


# Platforms

The following platform codes are available:

## Windows
**windows-32** (aliases: win, windows, win-32), **windows-64** (alias: win-64)

## Linux
**linux-32** (alias: linux), **linux-64**, **linux-armhf** (alias: linux-arm), **linux-armel**

## OS X
**osx-64** (aliases: mac, osx, mac-64)

You can use aliases as your platform code argument in both CLI and programatically.

# Included components

|          | Mac | Linux | Windows |
|----------|-----|-------|---------|
| ffmpeg   | v   | v     | v       |
| ffprobe  | v   | v     | v       |
| ffserver | v   | v     |         |
| ffplay   | v   |       | v       |


# Usage

You can run it from your code or through CLI.

If `output` argument is specified the binary will be placed there.
In CLI it will default to current working directory.
Programatically the default is the also the working directory.

If `platform` argument is missing then the current platform will be autodected and binaries for it will be downloaded.

If `components` argument is missing then binaries of all available components will be downloaded (see Components section).


## CLI

When installed globally with `npm i ffbinaries -g` this module will register
itself on command line interface.

### Arguments

CLI uses the following syntax: `ffbinaries [platform] [--components] [--output] [--quiet]`

### Examples

```
ffbinaries
ffbinaries mac
ffbinaries win-64 --quiet --components=ffplay
ffbinaries linux-64 -q --v=3.2 --c=ffmpeg,ffprobe --output=/home/user/ffmpeg
```

There are also `ffbinaries help`, `ffbinaries versions` and `ffbinaries clearcache`.


## Programatically

### Methods

`ffbinaries` library exports the following methods:

* `downloadFiles(platform, output, callback)` downloads and extracts the requested binaries.

* `getVersionData(version, callback)` fetches the full data set without downloading any binaries.

* `listVersions(callback)` returns the list of available versions from the API

* `listPlatforms()` returns the list of available platforms

* `detectPlatform()` returns the platform code of the machine as detected by the module.

* `resolvePlatform(input)` resolves input to a platform code (matches aliases).

* `clearCache()` purges local cache


### Example

```
var ffbinaries = require('ffbinaries');
var platform = ffbinaries.detectPlatform();
var dest = __dirname + '/binaries';

ffbinaries.downloadFiles(function () {
  console.log('Downloaded binaries for ' + platform + '.');
})

ffbinaries.downloadFiles('linux', function () {
  console.log('Downloaded binaries for linux.');
})


ffbinaries.downloadFiles('win-64', {components: ['ffprobe'], quiet: true, destination: dest}, function () {
  console.log('Downloaded ffprobe binary for win-64 to ' + dest + '.');
})
```
