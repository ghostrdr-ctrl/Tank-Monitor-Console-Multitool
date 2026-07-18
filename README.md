# Tank Monitor Console Multitool

A Windows desktop tool for backing up, comparing and restoring the programming in TLS-350 tank
monitor consoles, over the console's TCP/IP card or a serial-to-TCP bridge.

**[Download the latest version](../../releases/latest)** and run it. There is nothing to install.

## What it does

**Back up** a console's setup and save it. Reads the console's programming and writes either a
restorable backup (`.vrset`) or a plain readable report, or both from the same read. The backup
path cannot write to a console: it is built so that it can only ever issue read commands.

**Compare** two backups, offline, and see exactly what differs. Useful before and after a service
visit, or between two consoles that are supposed to match.

**Restore** a saved backup into a console. This is the one part that writes. It shows you a full
preview first and writes nothing until you confirm, it takes its own rollback backup of the
console before touching anything, and it reads every value back afterwards to check what actually
landed. You can restore just one area (for example, everything except Communications) when
cloning onto a different console.

**Program** a console's setup offline. Build the programming on your laptop with no console
present, save it as a backup file, and load it later through the same guarded restore.

**Find a console** on the network when you do not know its address.

**Live status.** With a console connected, the top of the window can show its current alarm state,
refreshed every few seconds. This only reads.

## Requirements

Windows, and a network route to the console (its TCP/IP card, typically port 10001, or a
serial-to-TCP bridge). Nothing else: the download is a single self-contained `.exe`.

Supports **TLS-350** consoles. TLS-450 and TLS-450PLUS are **not** supported — that path was never
verified on real hardware, so rather than offer something untested against leak-detection
equipment, the tool refuses those models outright.

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
- A backup can be incomplete if the connection drops. The tool marks those files as partial —
  do not treat one as a full copy of a console.
- A certified technician must verify leak-detection operation after any write.

Use at your own risk.

## Security

How to report a vulnerability, what the update checks do and do not guarantee, and what the tool
sends over the internet (almost nothing): see [SECURITY.md](SECURITY.md).

Each release publishes the SHA-256 of the `.exe` in its notes, so you can check that the file you
downloaded is the file that was published.

## Legal

This is an independent tool. It is **not developed, approved, endorsed, or supported by Gilbarco
Veeder-Root, its parent, or any of its affiliated companies.**

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

Developed by Josh B.
