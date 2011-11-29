---
layout: post
title: "Flickr Interestingness Downloadr"
date: 2011-04-18 12:00
comments: true
published: true
categories:
 - code
 - photography
---

[flickr]: http://www.flickr.com
[ness]: http://www.flickr.com/explore/interesting/7days/
[flickrpy]: http://code.google.com/p/flickrpy/
[flickrpyscript]: http://flickrpy.googlecode.com/svn/trunk/flickr.py
[createapp]: http://www.flickr.com/services/api/

I use the standard MacOS gallery screensaver with a large collection of
high-resolution photos which I've been collecting for several years. My
primary source of photos is [Flickr][flickr], and in particular the
[Interestingness / Last 7 Days][ness] page is a jackpot of great images. As
much fun as sitting around clicking `RELOAD!` is, I wanted to make the task a
bit more efficient.

I wrote a script `interestingness.py` (shown below) that downloads the last 24
hours worth of photos from the *interestingness* Flickr category.
Additionally, it only saves the largest available version of each image. Once
the images are all in a single directory, I open them all up in *Preview* and
use `Cmd-Backspace` to remove the images I don't want. After that, the
contents of the directory can be moved to the screeensaver archive folder.

To use the script you'll need a Flickr API access key, and the Flickr API
Python library.

#### Flickr API Key

1. Sign up for a Flickr account (it's free) at [flickr.com][flickr].
2. Request a [Flickr API key][createapp].
3. Save the *API Key* and *API Secret* (you'll need them for the next step).

#### Flickr Python Library

1. Download [`flickr.py`][flickrpyscript] from the [flickrpy][flickrpy] project.
2. Edit `flickr.py`: set the variables `API_KEY` and `API_SECRET` near the top of the file.

### Running the Script

1. Copy and paste the script below to a file called `interestingness.py`
2. Run as `python interestingness.py`
3. Enjoy (tested on MacOS and Linux)

{% codeblock interestingness.py %}

import sys
import flickr
from subprocess import Popen, PIPE, STDOUT

def _curl_get(url, verbose=False):
    p = Popen(["curl", "-O", url], stdout=PIPE, stderr=STDOUT)
    if verbose:
        sys.stdout.write(p.stdout.read())

def _wget_get(url, verbose=False):
    p = Popen(["wget", url], stdout=PIPE, stderr=STDOUT)
    if verbose:
        sys.stdout.write(p.stdout.read())

def _check_downloader(f, name):
    test_url = "http://www.google.com/index.html"
    try:
        print "Checking for "+name+" support..."
        f(test_url)
        print "..."+name+" supported"
        return f
    except:
        print "..."+name+" not supported"
    return None

_downloader = None

if not _downloader:
    _downloader = _check_downloader(_wget_get, "wget")
if not _downloader:
    _downloader = _check_downloader(_curl_get, "cURL")

if not _downloader:
    raise Exception("Please install cURL or wget")

def _choose_largest(photo):
    specs = photo.getSizes()
    specs = map(lambda s: (s['height'], s['width'], s['source']), specs)
    specs = sorted(specs, key=lambda s: s[0]*s[1], reverse=True)
    return specs[0][2]

if __name__ == '__main__':
    photos = flickr.interestingness()
    for photo in photos:
        url = _choose_largest(photo)
        _downloader(url, verbose=True)

{% endcodeblock %}
