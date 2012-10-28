Geany for Mac OSX
=================

The files in this directory are used to create the Mac Application bundle
for Geany. Bundles contain all the dependencies, configuration, themes
and so on that are needed to run Geany.

![Geany on Mac OSX Mountain Lion](https://github.com/trongthanh/geany-osx-essentials/raw/master/screenshot.png "Geany on Mountain Lion")

Compilation guide
-----------------

Without repeating all kinds of documentation that is already available
elsewhere, the basic process is:

1. Download and run the [`gtk-osx-build-setup.sh`][gtk-osx-build] script.  
    This prepares the jhbuild environment for building
2. Checkout this repo into submodule 'osx' of main Geany source.
3. Copy the `jhbuildrc-custom` to `~/.jhbuildrc-custom`, 
    over-writing the existing files.
4. Run jhbuild to get a OSX compatible GTK+ install with all needed
    dependencies. To do this, run these commands:

    ```shell  
    $ jhbuild bootstrap
    $ jhbuild build meta-gtk-osx-bootstrap
    $ jhbuild build meta-gtk-osx-core
    ```
5. Download, compile and install the Murrine GTK+ engine into the
    jhbuild $PREFIX. This is optional, but allows for better themeing.
6. Download, compile and install the [VTE library][vtelib] into the jhbuild 
    $PREFIX. This is optional, but allows for Terminal support in Geany.
7. Use `jhbuild shell` to setup environment for building Geany.
8. Compile and install Geany into the jhbuild $PREFIX.
9. Compile and install Geany plugins into the jhbuild $PREFIX.
10. Download and install: [`gtk-mac-bundler`][gtk-mac-bundler]
11. Change to the `osx/` directory in Geany's source tree (the one this file 
    is in) and create the application bundle by running:

    ```shell
    $ jhbuild shell
    $ export PATH=$PREFIX/bin:~/.local/bin:$PATH
    $ gtk-mac-bundler geany.bundle
    ```

Note: prepare to be frustrated ;)

Files details
-------------

Here's what the files in this directory do:

### Geany.icns

Geany's icons converted to Mac format. This format has the various icon
sizes all within one file.

### Info-geany.plist

Describes the app to the OS. Contains information like bundle name,
copyright and so on.

### geany.bundle

Settings file for gtk-mac-bundler to know how to create the application
bundle. This tells for example, which files to copy into the bundle.

### geany.conf

Default Geany configuration file for when one isn't in ~/.config/geany
already. This is a bit unusual but it allows to set some more appropriate
defaults for OSX.

### keybindings.conf

Default Geany keybindings to be more typical Mac keybindings, ex. where
the default for Save is Ctrl+s, this file changes that to Command+s.

### gtkrc

Customized GTK+ resource file to make Geany appear more appropriate on
OSX, like fonts, colours, etc. This theme is copied from the Greybird
theme used in Xubuntu (and elsewhere on Linux).

### jhbuildrc-custom

Config files for jhbuild to ensure the proper building.
  - These should be copied to ~/.jhbuildrc-custom before
    the first run of jhbuild so that it builds for the right architecture
    and so on.

### launcher.sh

Shell script copied into bundle that actually gets called first and
prepares the environment for running the real Geany binary. Anything
that needs to be done before running Geany can be done in here.

Binary Downloads
-------------------

The builds in downloads section are still experimental. There is no guarantee that they will run on your Mac. (Require Mac OSX 10.7+ 64-bit)
Extra plugins (some but not all) were compiled and bundled.

Known issues:
- Terminal panel not working due to no vtelib.so, a fix is in progress
- No Mac (global) menu. (Menu is still attached to window)
- Some keybindings don't work. For e.g. F7, F11...

Authors
------
- Matthew Brush &lt;matt(at)geany(dot)org&gt;: [Original author][original-repo]
- Trong-Thanh Tran &lt;trongthanh(at)gmail(dot)com&gt;: Update and build with Mountain Lion

[gtk-osx-build]: https://live.gnome.org/GTK%2B/OSX/Building
[vtelib]: http://www.linuxfromscratch.org/blfs/view/svn/xfce/vte2.html
[gtk-mac-bundler]: https://live.gnome.org/GTK%2B/OSX/Bundling
[original-repo]: https://github.com/codebrainz/geany/tree/wip/gtk-osx
