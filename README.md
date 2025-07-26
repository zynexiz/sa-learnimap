# sa-learnimap

**Train SpamAssassin using messages stored in IMAP "spam" folders across multiple mail accounts.**

This Python script connects to one or more IMAP accounts, searches for a spam folder (e.g. "Spam", "Junk", etc.), and pipes all found messages to `sa-learn --spam`, allowing SpamAssassin to improve its filtering accuracy based on actual user-classified spam.  

This is especially useful for mail servers like **Axigen Mail**, where messages are stored in internal databases rather than as individual files, making direct file-based training infeasible.

## Features

- Automatically detects spam folders (`Spam`, `Junk`, `Junkmail`, case-insensitive).
- Reads user credentials from a CSV file.
- Works over IMAP (with SSL).
- Calls `sa-learn` for each message.
- Optionally deletes messages after training (`--delete`).

## CSV Format

The script expects a CSV file with the following headers:

```csv
email,password
user1@example.com,secret123
user2@example.com,anotherpass
```
Store the CSV file somewhere safe! It contains plaintext passwords.

## Automate with systemd

To automatically run the script every night at 03:00, create the following two systemd files:

`/etc/systemd/system/sa-learnimap.service`
```
[Unit]
Description=Run SpamAssassin IMAP training script

[Service]
Type=oneshot
ExecStart=/usr/local/bin/sa-learnimap --users /etc/sa-learnimap/users.csv
User=someuser
Nice=10
```

`/etc/systemd/system/sa-learnimap.timer`
```
[Unit]
Description=Daily SpamAssassin IMAP training at 03:00

[Timer]
OnCalendar=*-*-* 03:00:00
Persistent=true

[Install]
WantedBy=timers.target
```
