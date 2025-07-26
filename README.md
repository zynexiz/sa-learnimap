# sa-learnimap

**Train SpamAssassin using messages stored in IMAP "spam" folders across multiple mail accounts.**

This Python script connects to one or more IMAP accounts, searches for a spam folder (e.g. "Spam", "Junk", etc.), and pipes all found messages to `sa-learn --spam`, allowing SpamAssassin to improve its filtering accuracy based on actual user-classified spam.

---

## Features

- Automatically detects spam folders (`Spam`, `Junk`, `Junkmail`, case-insensitive).
- Reads user credentials from a CSV file.
- Works over IMAP (with SSL).
- Calls `sa-learn` for each message.
- Logs useful output for each account (messages found, errors, etc.).

---

## CSV Format

The script expects a CSV file with the following headers:

```csv
email,password
user1@example.com,secret123
user2@example.com,anotherpass
