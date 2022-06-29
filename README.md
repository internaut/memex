# memex – encrypted chronological note keeping tool for unix CLIs

Markus Konrad <post@mkonrad.net>, June 2022

## Description

The *memex* tool is a bash script that can be used on almost any Unix-like OS for chronological note keeping on a day-by-day basis. It enables you to realize (daily) note keeping and recalling [as described by Cory Doctorow](https://doctorow.medium.com/the-memex-method-238c71f2fb46) based on the ideas of a "memory expanded" by [Vannevar Bush](https://www.w3.org/History/1945/vbush/) in 1945.

Your notes will be encrypted with your private GPG key to keep them safe even when you store them in an untrusted environment (e.g. a cloud space or an unencrypted flash drive). Note however that reading and editing requires temporary storage of unencrypted data so you should read and write notes only in trusted environments.

Memex allows to periodically review past notes by retrieving notes from previous dates on fixed time-deltas, e.g. "one week ago", "one month ago", "one year ago", etc. to form a *"powerful mnemonic"* (Doctorow). You can also add attachments (i.e. any files like images, videos, etc.) to each day's note – these will also be encrypted.

## Requirements

- a Unix-like operating system with a bash shell and standard tools `date`, `tar`, `grep`, `gpg`, `vim` and optionally `xdg-open` if you want to open note attachments 
- a gpg keypair; generate one if you haven't so far; keep note of the identity email used in your gpg keypair

## Setup

- make sure the `memex` script file is executable (`chmod +x memex`)
- edit the `memex` script file to set the following to variables right at the beginning of the script:
  - `IDENT`: identity email used in your gpg keypair
  - `TMPDIR`: local, trusted temporary folder; must be writable; unencrypted data will be written temporarily to this location
- memex will use your default editor specified by the `EDITOR` environment variable; if that's not given it will use `vim`; you can also hardcode an editor to use by editing the `EDIT` variable

## Usage

Interact with `memex` using the commands described below. `<...>` denotes required arguments, `[...]`  denotes optional arguments. The `date` argument is very important. It allows you to specify for which date you want to read or write a note. The way you specify that date is very flexible: You can specify the day in "year-month-day" format (e.g. "2022-06-29") or in a relative way, e.g. "2 days ago", "3 weeks ago" or "last friday". See the [documentation for the Unix `date` utility](https://www.gnu.org/software/coreutils/manual/html_node/Date-input-formats.html) for more information.

All data is stored in the sub-folder "memexdb", where each day has a folder named in the "year-month-day" (YYYY-MM-DD) format. Inside these folders the encrypted data is stored as `YYYY-MM-DD.tar.gpg`. The encrypted data contains at least one text note file named `YYYY-MM-DD.txt` and optional attachment files.

### Commands

- `./memex` or `./memex write [date] [attach]`: create a new note or edit a note for "date" or today (default); if "attach" is given, open folder to add attachment files
- `./memex read [date]`: if date is given, read the note at that date; else read notes for a sequence of past dates
- `./memex search <pattern>`: search (encrypted) notes for a given grep pattern; this may take some time since all notes need to be encrypted one by one; decrypted data is directly piped to `grep` so no temporary unencrypted data is stored
- `./memex last`: show the date of the last note that was taken
- `./memex encrypt`: encrypt all note data in the memexdb that is not encrypted so far; the unencrypted notes must be named in the "YYYY-MM-DD.txt" format inside the respective days' folders; if all data is encrypted the command will not report anything

