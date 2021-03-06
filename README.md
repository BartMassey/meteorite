#Meteorite Matroska/MKV Repair Engine

Meteorite is a tool to fix broken MKV files.

This is a fork of an abandoned (I believe) project from SourceForge.
See [Documentation from original website](#documentation-from-original-website)
below for full details.

* Version 0.20+ Copyright (C) 2016 Andrew Barnert
* Version 0.12 Copyright (C) 2009 Erdem U. Altinyurt
* Licensed under GPL version 2.0 or later. See docs/GPL.txt for details.

#Build

The Meteorite command line interface (CLI) version only requires a C++ compiler to 
build (g++ 4.6 works well - also on Windows).
The Meteorite GUI requires [wxWidgets][wx], probably at least 2.8 or 3.0.
Tested with version 3.0 and 3.1, installed via `brew install wxmac` 
and Windows installer. There may be other prereqs; I honestly don't 
know yet.

  [wx]: https://www.wxwidgets.org/

* `make cli` builds only `meteorite-cli[.exe]`.
* `make` builds two executables, `meteorite` and
  `meteorite-cli`, which will run from the source tree.
* `[sudo] make install` may work on some platforms, but I don't
  know how far I'd trust it. (It does handle the usual `$DESTDIR`, 
  `$PREFIX`, etc. customizations.) I definitely wouldn't use it on Mac.
* `make mac` will build a `.app` bundle, which can then be copied
  into your applications directory. (However, at present, you'll
  probably want to run it from a terminal anyway.)
* `contrib/meteorite.spec` can presumably be used for building an
RPM package.

#Use (including known issues)

Currently, the build produces two separate executables.

##CLI

The CLI can be run directly from the build directory, or from the
install directory.

    meteorite-cli FILENAME [FILENAME]*

For each `FILENAME`, this creates a repaired file, prefixed with
`Meteorite.`, in the same directory. There are no flags or options.
So, for example, if you fix `~/Movies/spam.mkv`, you will get
`~/Movies/Meteorite.spam.mkv`.

Currently, there's a whole lot of debug information, dumped to both
stderr and stdout.
Use piping of std-out to get rid of most verbose messages, i.e.
    
    meteorite-cli FILENAME [FILENAME]* >log.txt

At the end of each run, you should get a
banner like this:

    ************************************************************
    SUCCESS: src -> /Users/andi/test.mkv -> /Users/andi/Meteorite.test.mkv
    ************************************************************

There may be a bug that causes it to hang at completion, but I believe
this only affects the GUI build.

You should be able to safely interrupt a repair with `^C` (although
this will leave a useless incomplete destination file).

##GUI

The GUI can be run directly from the build directory as `meteorite`,
or (on Mac) by opening the `meteorite.app` bundle. There are no
command-line arguments.

Launching the app displays a drop target. Drag one or more files to
the drop target, and the app should behave the same as if you'd passed
the same files to the CLI.

There's no GUI feedback. This means you will probably want to run the
app from the terminal. On Mac, if you've built an an app bundle, this
should be something like `./meteorite.app/Contents/MacOS/meteorite` or
(if you've copied that bundle to the Applications directory)
`/Applications/meteorite.app/Contents/MacOS/meteorite`.

There's also no CLI feedback except for the debug
information. Usually, the last piece of information will be a dump of
the final structure, which should end with a line similar to this:

    |-Void: 00000000000000000000: size 5035

If you're not sure whether the repair has completed, quit the app, to
make sure the file is flushed and closed, and now it should play.

You can quit the app before it's done, but it will probably segfault.

Note that on the Mac version, the menu bar is broken. This means you
can't quit from the `Meteorite` menu, or with the usual Cmd-Q
shortcut. (Although, oddly, there is a context menu that works.) Just
close the window to quit.

Sometimes, at completion, the app hangs. Killing it (`^C` from the
terminal, or `kill` (no `-9` needed), or your favorite GUI tool is
safe; any output files have been flushed. I don't know whether this is
related to a warning that gets logged by `CoreAnimation` on the Mac
about ending a thread with an open transaction, but it certainly looks
suspicious.

#Roadmap

Note that, as of version 0.30, the app pretty much works well enough
for me, so I may not get back to work on it again until I get really
annoyed by the output spam...

* Version 0.20 (done, 2016-07-11)
 * Fork original Sourceforge repo.
 * Get the code compiling again.
 * Fix the startup crash.
* Version 0.30 (done, 2016-07-11)
 * Create a version that works as a command-line tool.
* Version 0.31
 * `getopt_long` to control logging levels, specify output dir, etc.
 * Windows CLI version.
 * Clean up logging.
 * Fix hang on completion bug.
* Version 0.32
 * Check for interactive TTY on CLI startup.
 * Improve non-interactive CLI output.
 * Add interactive CLI feedback (progress spinner).
* Version 0.40
 * Add interactive GUI feedback.
 * Add basic options in GUI.
 * Make menu bar work in GUI.
 * Make cancel not crash in GUI.
* Future
 * Make wx optional for building.
 * Ideally merge two executables into one when built with wx.
 * Make GUI also handle command-line input.
 * Some way to specify validate/dry-run.
 * Food in pill form.

#Original version information

Copyright (C) 2009  Erdem U. Altinyurt

The original project at [mkvrepair.com][mkvrepair] (and [sourceforge][sf])
seems to have stalled. The latest binary release, version 0.11 beta, crashes
on at least some platforms. The author checked in a fix and said version
0.12 would be coming soon, but the checked-in code won't compile. The last
update on SF was on 2012-06-27; the last update to the website says
"Please help to spread Meteorite for resuming development", and has a link
to donate.

  [mkvrepair]: http://www.mkvrepair.com/
  [sf]: https://sourceforge.net/p/meteorite

#Documentation from original website

Meteorite Project is [DivFix++][divfix] like program but for Matroska/MKV files.

It can repair your corrupted MKV video files to make it compatible with your player.

Also you can preview Matroska files those are already in download.

##High Definition Video

Why I made this program? Because I cannot watch files which are currently on download from p2p networks like emule,torrent.
Also, new videos, specially in HD, high definition videos are in MKV format generally.
You needed to fix them before watch. But unfortunatelly there was no program could fix matroska files.
So I make this tool, just for myself than released source and binary on my birthday...
You can repair your half downloaded MKV / Matroska videos or broken movies with it too.

  [divfix]: http://divfix.org/
