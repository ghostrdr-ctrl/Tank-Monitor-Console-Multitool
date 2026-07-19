# Security policy

## Reporting a vulnerability

Please report security issues **privately**, not as a public issue.

Use GitHub's [private vulnerability reporting](../../security/advisories/new) on this repository.
If that is unavailable, open a normal issue saying only that you have a security report and asking
for a contact. Do not put any details in the public issue.

Please include what you found, how to reproduce it, and what an attacker could achieve. You will
get an acknowledgement, and a fix or an explanation of why it is not being changed. This is a
small project maintained by one person, so please allow reasonable time.

## Scope

This tool reads and writes the programming of fuel-tank monitoring consoles, which governs leak
detection and regulatory compliance. Reports that matter most:

- anything that could make the tool **write to a console when it should not**, or write something
  other than what the operator confirmed
- anything that defeats the **update checks** described below
- anything that could make a backup silently incomplete or a restore silently partial while
  reporting success

Not in scope: the fact that Windows SmartScreen warns on the download (see below), and the
accepted limitation of self-update authenticity, also described below.

## How updates are secured, and what that does not cover

The tool checks this repository's releases for a newer version, and can install one when the
operator says yes. Deliberate properties:

- The browser is only ever sent to this repository's releases page, a **constant compiled into
  the program**, never to a URL taken from the API response. A spoofed or altered reply cannot
  redirect anyone.
- A download is accepted only if its URL sits under this repository's own
  `releases/download/` namespace, re-validated immediately before writing to disk, and the final
  URL after redirects must still be HTTPS.
- If more than one, or no, installable file is offered, the tool refuses rather than guessing
  which executable to run.
- The download is checked for a valid Windows executable header, a sane size, and the size the
  release reports, before anything is replaced.
- Installing is refused while the tool is communicating with a console, and the previous version
  is kept so a bad update can be rolled back by hand.
- Any failure to check for updates is silent: the tool carries on working offline.

**What this does not prove: authenticity.** The size and hash used to check a download come from
the same release data as the download URL, so they detect a corrupt or truncated file, not a
malicious one. Authenticity therefore rests on HTTPS and on control of this GitHub account. The
executable is Authenticode-signed, but with a **self-issued certificate**, so verifying that
signature only proves the file was signed by whoever produced it. It is not backed by a
certificate authority. In practice this means **a compromised release, or a compromised account,
would be installed by anyone who clicks yes.**

Each release publishes the SHA-256 of the executable in its notes so you can confirm the file you
downloaded matches what was published.

If the tool is deployed somewhere this risk is unacceptable, do not use the self-update feature:
download releases manually and verify them under your own change control.

## Windows SmartScreen

The download is signed, but with a self-issued certificate, so Windows will still show
"Windows protected your PC" the first time you run each new version. That is Windows saying it
does not recognise the publisher, not that it found anything wrong with the file. Only download
releases from this repository's releases page.

## Privacy

The tool contacts the internet for exactly one thing: asking this repository whether a newer
release exists. It sends no information about you, your site, or any console: no telemetry, no
analytics, no usage reporting. Everything else it does is between your computer and the console
on your own network.
