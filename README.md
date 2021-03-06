# Compton

__Compton__ is a compositor for X, and a fork of __xcompmgr-dana__.

I was frustrated by the low amount of standalone lightweight compositors.
Compton was forked from Dana Jansens' fork of xcompmgr and refactored.  I fixed
whatever bug I found, and added features I wanted. Things seem stable, but don't
quote me on it. I will most likely be actively working on this until I get the
features I want. This is also a learning experience for me. That is, I'm
partially doing this out of a desire to learn Xlib.

## Changes from xcompmgr:

* __inactive window transparency__ (specified with `-i`)
* __titlebar/frame transparency__ (specified with `-e`)
* menu transparency (thanks to Dana)
* shadows are now enabled for argb windows, e.g. terminals with transparency
* removed serverside shadows (and simple compositing) to clean the code,
  the only option that remains is clientside shadows
* configuration files (specified with `--config`)
* colored shadows (with `--shadow-[red/green/blue] value`)
* a new fade system
* vsync (still under development)
* several more options

## Fixes from the original xcompmgr:

* fixed a segfault when opening certain window types
* fixed a memory leak caused by not freeing up shadows (from the freedesktop
  repo)
* fixed the conflict with chromium and similar windows
* [many more](https://github.com/chjj/compton/issues)

## Building

### Dependencies:

__B__ for build-time

__R__ for runtime

* libx11 (B,R)
* libxcomposite (B,R)
* libxdamage (B,R)
* libxfixes (B,R)
* libXext (B,R)
* libxrender (B,R)
* libXrandr (B,R)
* pkg-config (B)
* make (B)
* xproto / x11proto (B)
* bash (R)
* xprop,xwininfo / x11-utils (R)
* libpcre (B,R) (Will probably be made optional soon)
* libconfig (B,R) (Will probably be made optional soon)
* libdrm (B) (Will probably be made optional soon)
* libGL (B,R) (Will probably be made optional soon)
* asciidoc (B) (if you wish to run `make docs`)

### How to build

To build, make sure you have the dependencies above:

``` bash
# Make the main program
$ make
# Make the newer man pages
$ make docs
# Install
$ make install
```

(Compton does include a `_CMakeLists.txt` in the tree, but we haven't decided whether we should switch to CMake yet. The `Makefile` is fully usable right now.)

## Known issues

* VSync does not work too well. It's widely reported that tearing still happens on the top of the screen. I do not know how to fix the issue.

* If `--unredir-if-possible` is enabled, when compton redirects/unredirects windows, the screen may flicker. Using `--paint-on-overlay` minimizes the problem from my observation, yet I do not know if there's a cure.

* compton may not track focus correctly in all situations. The focus tracking code is experimental. `--use-ewmh-active-win` might be helpful.

* Compton may give ugly shadow to windows with ARGB background if `-z` is enabled, because compton cannot determine their real shapes. One may have to disable shadows on those windows with window-type-specific settings in configuration file or `--shadow-exclude`.

* There are two sets of man pages in the repository: the man pages in groff format (`man/compton.1` & `man/compton-trans.1`) and the man pages in Asciidoc format (`man/compton.1.asciidoc` & `man/compton-trans.1.asciidoc`). The Asciidoc man pages are much more up-to-date than the groff ones, and it is viewable online. As chjj has not yet expressed his attitude towards switching to Asciidoc man pages, I kept both versions. By default the groff version is installed, unless you run `make docs`.

* The performance of blurring is terrible, probably because of a problem in the X Render implementation. Its behavior is driver-dependent: With nvidia-drivers it works but there are strange 1px lines remaining when you operate on windows (not sure if it's a bug in compton or in the driver); with nouveau it's utterly broken.

## Usage

Please refer to the Asciidoc man pages (`man/compton.1.asciidoc` & `man/compton-trans.1.asciidoc`) for more details and examples.

Note a sample configuration file `compton.sample.conf` is included in the repository.

## Support

* Bug reports and feature requests should go to the "Issues" section above.

* Our (semi?) official IRC channel is #compton on FreeNode.

* Some information is available on the wiki, including (and presently, only includes) a FAQ.

## License

Although compton has kind of taken on a life of its own, it was originally
an xcompmgr fork. xcompmgr has gotten around. As far as I can tell, the lineage
for this particular tree is something like:

* Keith Packard (original author)
* Matthew Hawn
* ...
* Dana Jansens
* chjj and richardgv

Not counting the tens of people who forked it in between.

Compton is distributed under MIT license, as far as I (richardgv) know. See LICENSE for more info.
