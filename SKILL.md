---
name: sftp-directory-audit
description: Connect to a remote server over SFTP (read-only), go through a given directory including all subfolders, open every file (.py or any type) and tell only what each file does. Use this skill whenever the user wants to check, scan, or understand a directory on a remote server or VPS, shares SFTP/SSH details (host, port, user, directory path), or says things like "check my server folder", "ye file kya karti hai", "server ki directory check karo", "ek ek karke batao konsi file kya kaam karti hai".
---

# SFTP Directory Check

Connect to a remote server over SFTP, go through ONE directory the user names (all subfolders inside it too), open every file one by one, and tell only what each file does. Nothing extra.

## Hard rules

1. **Read-only.** Never upload, edit, rename, delete, or chmod anything. Only list and read.
2. **Stay inside the given directory.** Do not follow symlinks outside it or browse parent folders.
3. **Never save credentials** anywhere (files, logs, scripts on disk, final report). Use them only for the current session.
4. **Mask secrets.** If a file contains a password, key, or token, never print it in full. Say only that the file uses one.

## Inputs to collect

Ask only for what is missing: host, port (default 22), username, password or key path, and the absolute directory path.

## Prerequisites

Needs network access to the server. Works in Claude Code or Cowork on the user's machine. If network is blocked, say so in one line and stop.

## Workflow

1. Connect with `sftp`, `paramiko`, or `ssh`/`scp`. Pass the password at runtime only. Verify the host key.
2. List the directory recursively, including every subfolder.
3. Open every file one by one:
   - Code and text files (.py, .js, .php, .sh, .json, .yml, .conf, .html and so on): read them and work out what they do.
   - Binary, image, archive, or huge files: do not open the contents. Say what kind of file it is from its name and type.
   - Skip heavy folders (`node_modules`, `.git`, `venv`, `__pycache__`, caches) but name them with one line each.
4. Report each file.

## Output format

Group by folder. For each file give ONE short line, or two at most:

```
📁 folder/subfolder/
- file.py: kya karti hai (simple words)
- other.json: kya karti hai
```

## What NOT to do

- No advice, tips, fixes, or improvement suggestions.
- No security audit, risk flags, or severity levels.
- No long explanations, no history or background about the language or framework.
- No big summary at the end. One closing line with the number of files checked is enough.

Only say what the file does. If a file cannot be read, write "padh nahi payi (reason)" and move on.

## Language and tone

Reply in the user's language. If she writes Hinglish, answer in simple Hinglish. Address the user respectfully (aap). Plain words, no jargon.

## Edge cases

- **Connection fails:** report the error type in one line (auth failed, timeout, host key mismatch, permission denied). Do not retry in a loop.
- **Very large directory (over about 500 files):** go folder by folder and tell the user when each folder is done.
- **User asks to change something:** this skill only reads. Say so in one line.
