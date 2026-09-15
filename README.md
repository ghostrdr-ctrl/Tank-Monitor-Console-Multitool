# Tank Monitor Console Multitool

A Windows desktop tool for backing up, comparing, restoring and programming the setup in TLS-350
tank monitor consoles, over the console's TCP/IP card or a serial-to-TCP bridge. It also pulls
the console's own reports for a compliance file, and can carry a TLS-350's programming onto a
TLS-450PLUS.

**[Download the latest version](../../releases/latest)** and run it. There is nothing to install.

## What it does

**Back up** a console's setup and save it. Reads the console's programming and writes either a
restorable backup (`.vrset`) or a plain readable report, or both from the same read. The backup
path cannot write to a console: it is built so that it can only ever issue read commands.

A backup says what it does not contain. It records which areas of the setup it covered, and lists
any setting the console did not answer for, so a file that is missing part of a console cannot be
mistaken for a full copy of one. You can also back up a single area rather than everything, which
is much quicker when you only need one part.

**Compare** two backups, offline, and see exactly what differs. Useful before and after a service
visit, or between two consoles that are supposed to match.

**Pull the console's own reports** for a compliance file: leak test results and history, CSLD,
line leak, sensor and pump status, alarm history, inventory and deliveries. Over eighty reports to
choose from, with a one-click compliance pack that ticks the usual set, saved together in one text
file. They are stored exactly as the console prints them, word for word. This only reads, and the
tool does not re-format or re-interpret any value in them.

**Restore** a saved backup into a console. This is the one part that writes. It shows you a full
preview first and writes nothing until you confirm, it takes its own rollback backup of the
console before touching anything, and it reads every value back afterwards to check what actually
landed. You can restore just one area (for example, everything except Communications) when
cloning onto a different console.

It also compares the console's software version against the version the backup came from, and
stops if the console is older, because settings that version does not have would be accepted and
quietly dropped. You can override that if you know it is what you want. If the backup was taken
before a setting existed, or the console never answered for it, the restore says so up front
rather than reporting a clean result.

**Check a backup before you drive to site.** The Check file button answers "will this restore
work" with no console attached: whether the file would be refused, what would actually be written,
and what the file is missing.

**Roll back a restore.** The Restore tab finds the rollback file the last restore took for that
console, shows a preview first, then writes it back. It takes a fresh rollback of the console as it
is now before it writes, so the undo can itself be undone.

**Finish an interrupted restore.** Every write is recorded as it lands, so a restore that is cut
off part way can be resumed and sends only what did not go. Anything the console refused, or that
was sent with no reply, is sent again, and all the usual checks and confirmations run again.

**Program** a console's setup offline. Build a TLS-350 or TLS-450PLUS configuration on your laptop
with no console present, save it as a backup file, and load it later through the same guarded
restore. Every 450PLUS field says how well proven it is on real hardware, and a How well proven
button lists what is worth testing at a console and in what order.

**See what every backup on the laptop is.** The Files tab lists every backup and rollback file on
the computer, with what each one is and when it was read, and calls out any that must not be used
to restore a console: a backup whose read did not finish, one with no records in it, a read-only
reference sweep, and any file whose contents no longer match the stamp the tool wrote.

**Every file the tool writes is stamped.** A backup, a migrated file or a Program tab save records
which version wrote it and the oldest version allowed to use it, and is refused if it needs a newer
version than you are running, or if it has been altered since it was written. The stamp is there
to catch an edited or corrupted file, not to keep anyone out. Files from versions before 0.57.0
carry no stamp; they still work, with a note saying the check could not run.

**Find a console** on the network when you do not know its address. If the console cannot be
reached at all, because its address is on a different subnet from your laptop, the tool can listen
for it announcing itself: start listening, power-cycle the console, and it reports the address the
card claims. That works even when nothing can route to it. Installing Wireshark lets the tool
capture the announcement directly, which is the most reliable way; without it the tool falls back
to watching your PC's address cache and tells you so.

**Migrate a TLS-350 onto a TLS-450PLUS.** Its own tab, in five steps: pick the 350 backup, enter
the device addresses, migrate, load, and the line-disable and relay events are created for you
once the load finishes. The saved addresses can be reopened and corrected one box at a time. The
two consoles are not the same machine, so a straight copy is not possible: settings that mean
different things on the two are left out and listed for you, settings a real 450PLUS has been
measured to refuse or throw away are listed under their own heading so they can be entered on the
console, and the things a 450PLUS needs that a 350 has no equivalent for (device addresses,
products, pumps and lines) are built for you from what you enter. It produces one file to load,
and every report ends with what is still missing. The whole job takes one confirmation, typed as
the console's IP, after a single block that lists everything it is about to do.

**Live status.** With a console connected, the top of the window can show its current alarm state,
refreshed every few seconds. This only reads.

## Requirements

Windows, and a network route to the console (its TCP/IP card, typically port 10001, or a
serial-to-TCP bridge). Nothing else: the download is a single self-contained `.exe`.

Supports **TLS-350** consoles fully.

**TLS-450PLUS** is supported for backup, migration, offline programming and restore, and is
marked experimental throughout. It has been used to commission real 450PLUS consoles from bare and
read back to confirm what landed, but on only a couple of consoles, so treat every value it writes
as something to check rather than trust. The Restore tab asks you to confirm twice before it
writes to one; the Migration tab asks once, by typing the console's IP.

**TLS-450** (without PLUS) is refused outright. That path was never verified on real hardware, and
rather than offer something untested against leak-detection equipment the tool will not touch it.

## Updates

The tool checks the releases page when it starts and shows a strip across the top when a newer
version is out. There is also a **Check for updates** button on the About tab if you want to ask
at any time. If the check fails, or there is no internet, it stays quiet and the tool works
normally.

It can install the update for you: it downloads the new version, checks it, closes and reopens on
the new one. It only ever does that when you say yes. Nothing is downloaded or replaced in the
background, and saying no leaves your version exactly as it is.

**It will refuse to install an update while it is talking to a console.** Installing restarts the
program, and interrupting a restore could leave a console half-programmed. Finish the job first.

## Windows will warn you the first time

The tool is signed, but with a self-issued certificate rather than one from a commercial authority,
so Windows still shows a blue "Windows protected your PC" screen the first time you run it. Click
**More info**, then **Run anyway**. Your browser may also ask you to keep the download rather than
discard it.

The warning is Windows saying it does not recognise the publisher, not that it found anything
wrong with the file. Expect it again after an update, because each new version is a new file as
far as Windows is concerned.

The signature itself names **Verbose Software**, and so does the file's Properties > Details.
From version 0.53.0 that signature uses a new certificate. If your laptop was set up to trust
the old one, import the certificate published with the release to have Windows recognise the
publisher again.

Only ever download it from the [releases page](../../releases) linked above.

## Status

Beta, and under active development. It is a work in progress and may contain bugs.

**This tool talks to live leak-detection equipment.** Programming a tank monitor affects leak
detection and regulatory compliance. It is intended for trained service technicians working on
consoles they are authorised to service.

- The developer is not responsible for any missing, incorrect or lost configuration, for equipment
  downtime, for any compliance consequence, or for any other issue that may result from using this
  tool.
- Back up a console before writing to it, keep the rollback file the restore creates, and verify
  the console's programming afterwards.
- **Print the console's setup at the console, before and after using this tool, and compare the
  two printouts.** That check does not depend on this tool being correct, which is exactly why it
  is worth doing.
- A backup can be incomplete: the connection can drop part way, a console can fail to answer a
  read, or the backup can have been taken over one area of the setup rather than all of it. The
  tool records all three in the file and repeats them when you restore it. Backups taken with
  older versions do not carry that record, or the stamp that would show they have not been
  altered, so check an old file before relying on it.
- A TLS-450PLUS backup is not a substitute for the console's own DB Backup. Users, roles,
  passwords, vapour recovery, printer setup and feature activation cannot be reached over the
  serial port at all, so a file from here can never restore one of those consoles from clean.
- A certified technician must verify leak-detection operation after any write.

Use at your own risk.

## Security

How to report a vulnerability, what the update checks do and do not guarantee, and what the tool
sends over the internet (almost nothing): see [SECURITY.md](SECURITY.md).

Each release publishes the SHA-256 of the `.exe` in its notes, so you can check that the file you
downloaded is the file that was published.

## Legal

This is an independent tool, developed and published by Verbose Software. It is **not developed,
approved, endorsed, or supported by Gilbarco Veeder-Root, its parent, or any of its affiliated
companies.**

TLS-350 is a trademark of Gilbarco Veeder-Root. All other product names and trademarks are the
property of their respective owners, and are used here only to describe what the tool works with.

## Licence

Free to download and use, including at work. It is **not** open source: the source is not
published, and the tool may not be sold or passed off as someone else's work. See
[LICENSE](LICENSE) for the terms and [THIRD-PARTY-NOTICES.md](THIRD-PARTY-NOTICES.md) for the
open-source components bundled inside the download.

## Notes

Changes in each release are listed on the [releases page](../../releases). The source is kept in
a separate private repository.

Developed and published by Verbose Software.

Copyright (c) 2026 Verbose Software. All rights reserved.
