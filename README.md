# ghvolo

A command line tool that uses volo as a library to do searches
and resolutions of github IDs for front end/browser-based web
dependencies.

This shows how you could use volo's github awareness just
for identifying code on github, without using volo's other
features.

This may be useful if you like the idea of using github as
a package registry, but did not want to use the volo client
tool to pull down the code.

It also allows you to experiment with how different IDs
would be resolved using github and its API for a registry.

These features of volo do not require or assume AMD or some
other module format to work. It just operates on github,
using `owner/repo` pairs for final resolution, and version
tags to find the latest release.

Running ghvolo does not install anything, it just prints out JSON
results.

## Install

Install globally so that the bin command works in any directory.

    npm install -g ghvolo

## Usage

There are two commands that ghvolo understands:

### search

Search github for a given repo name:

    ghvolo search jquery

This will give the top five github search results for repos that are
language=JavaScript. The first search result's `owner/repo` combination is
used for the `resolve` command below.

### resolve

Resolves a repo name to the `owner/repo/latestVersionTag` ID,
along with info on a download URL. Understands package.json
files that have a `volo` or `browser` section, and will fall
back to [volojs/repos](https://github.com/volojs/repos) for shim
config for any `owner/repo` pairs that are in volojs/repos:

    ghvolo resolve jquery

### Raw JSON output

By default, ghvolo uses prettyjson to show the JSON output. However, if
you are more comfortable reading the raw JSON, pass `-j`:

    ghvolo -j search jquery
    ghvolo -j resolve jquery

## Sample commands to try

    ghvolo resolve bootstrap#js/bootstrap-alert.js

Resolves 'bootstrap', and give an url for the latest tag version, picking
out just the `js/bootstrap-alert.js` file within that tagged archive.

    ghvolo resolve backbone

Resolves backbone. documentcloude/backbone has a shim config in volojs/repos,
so volo dependencies are listed in the packageInfo section, and since the
volojs/repos shim has a `volo.url` section, a direct download link for
backbone.js is used for the download URL.

    ghvolo resolve underscore

Resolves to documentcloud/underscore. underscore does not have a
shimmed package.json for a simple one file JS download url, so the download
URL is a zip file for the latest release version.

    ghvolo resolve html5boilerplate

Resolves to h5bp/html5-boilerplate and gives a zipball URL for the latest
version tag release.

