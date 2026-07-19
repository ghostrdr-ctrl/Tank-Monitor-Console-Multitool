# Third-party notices

The Tank Monitor Console Multitool download is a single self-contained `.exe`. So that it runs
without asking you to install anything, the download bundles a frozen copy of a Python runtime and
the components below. Each remains under its own licence and its own copyright, held by its
respective authors. They are listed here because the download redistributes them in binary form.

The tool itself uses only the Python standard library. There are no third-party Python packages
in it.

| Component | Version | Licence |
|---|---|---|
| [Python](https://www.python.org/) | 3.14.4 | [PSF License Agreement](https://docs.python.org/3/license.html) |
| [Tcl/Tk](https://www.tcl.tk/) | 8.6.15 | [Tcl/Tk License (BSD-style)](https://www.tcl.tk/software/tcltk/license.html) |

Tcl/Tk is included because the program's window is built with Tkinter, the standard library's
interface to Tk; PyInstaller bundles the Tcl and Tk runtimes automatically.

The executable is packaged with [PyInstaller](https://pyinstaller.org/) 6.21.0, whose bootloader
is included in the download. PyInstaller is GPL-2.0-or-later **with a special exception that
expressly permits using it to build and distribute non-free programs**, including commercial
ones, which is the basis on which it is used here.

These components are the property of their respective authors and are not covered by the
`LICENSE` that applies to the Tank Monitor Console Multitool itself. Their licences continue to
govern them. Full licence texts are available at the links above.
