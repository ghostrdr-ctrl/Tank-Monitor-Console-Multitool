# Security policy

## Reporting a vulnerability

Report security issues privately through GitHub's
[private vulnerability reporting](../../security/advisories/new). If that is unavailable, open an
issue asking for a contact, with no details in it.

Include what you found, how to reproduce it, and what an attacker could do with it.

## Scope

Most important:

- anything that makes the tool write to a console when it should not, or write something other
  than what the operator confirmed
- anything that defeats the update checks below
- anything that makes a backup or restore look complete when it is not

Not in scope: the Windows SmartScreen warning on download, and the self-update limitation below.

## Updates

- The tool checks this repository's releases, and installs an update only when the operator says
  yes, and never while connected to a console.
- The only page it opens is this repository's releases page, built into the program.
- A download is accepted only from this repository's `releases/download/` address, over HTTPS,
  and only if exactly one `.exe` is offered.
- The download is checked for a Windows executable header, its size and its SHA-256 before
  anything is replaced. The previous version is kept.
- If the check fails or there is no internet, the tool carries on offline.

**Authenticity is not proven.** The size and hash come from the same release data as the download,
so they catch a corrupt file, not a malicious one. The executable is signed with a self-issued
certificate, not one from a certificate authority. A compromised release or account would be
installed by anyone who says yes. Where that risk is unacceptable, do not use self-update: download
releases yourself and check the SHA-256 published in each release's notes.

## Privacy

The tool contacts the internet only to check this repository for a newer release. It sends nothing
about you, your site or any console: no telemetry, no analytics.
