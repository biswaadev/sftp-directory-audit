# SFTP Directory Audit Skill 🔍

An AI agent skill designed to safely connect to remote servers over SFTP (read-only), inspect directories recursively, and explain exactly what each file does in concise, plain language.

Compatible with **Google Antigravity**, **Claude Code**, **Cowork**, and other AI coding agent environments supporting skill definitions.

---

## 🌟 Key Features

- **🔒 100% Read-Only Safety**: Strictly inspection-only. Never creates, modifies, renames, deletes, or changes file permissions.
- **🛡️ Automatic Secret Masking**: Detects sensitive tokens, API keys, database credentials, and passwords, masking them completely to prevent accidental leaks.
- **⚡ Crisp, No-Nonsense Explanations**: Delivers 1–2 line summaries describing the exact function of each file, avoiding unnecessary fluff, verbose lectures, or unsolicited refactoring advice.
- **🧹 Smart Directory Filtering**: Automatically detects and skips heavy dependencies and build artifacts (`node_modules`, `.git`, `venv`, `__pycache__`, cache folders), noting them concisely.
- **🗣️ Flexible Language & Tone**: Respects user preference—works seamlessly in English, Hinglish, or Hindi with a polite and direct tone.

---

## 📋 How It Works

1. **Input Collection**: Prompts only for missing credentials (Host, Port, Username, Password/SSH Key, and Remote Directory Path).
2. **Secure Connection**: Establishes an ephemeral SFTP/SSH session. Credentials are used strictly in-memory during the session and are never saved or logged to disk.
3. **Recursive Directory Traversal**: Navigates the target directory and subdirectories without straying outside the root target.
4. **Content Analysis**:
   - **Code & Text files** (`.py`, `.js`, `.ts`, `.sh`, `.json`, `.yml`, `.conf`, `.html`, etc.): Reads contents to deduce primary responsibility.
   - **Binary & Media files** (`.zip`, `.png`, `.so`, executables, etc.): Identifies file type by metadata without loading full binaries.
5. **Structured Report**: Produces a clean, folder-by-folder breakdown.

---

## 📊 Sample Output

```markdown
📁 project/src/
- auth.py: Handles user JWT authentication and password hashing
- database.py: Configures SQLAlchemy engine and database connection pool
- config.json: Application settings (contains masked API keys)

📁 project/src/utils/
- logger.py: Formats and routes application logs to stdout
- helpers.py: Helper functions for string formatting and date parsing

📁 project/node_modules/ (skipped - dependency folder)

✅ Checked 14 files across 3 directories.
```

---

## 🚀 Installation

### 1. Google Antigravity / Gemini CLI
Clone the skill into your personal skills directory:

```bash
# Global installation
git clone https://github.com/biswaadev/sftp-directory-audit.git ~/.gemini/skills/sftp-directory-audit

# Or workspace-specific installation
git clone https://github.com/biswaadev/sftp-directory-audit.git .agents/skills/sftp-directory-audit
```

### 2. Claude Code / Other Agent Frameworks
Clone into your agent's skills path:

```bash
git clone https://github.com/biswaadev/sftp-directory-audit.git ~/.claude/skills/sftp-directory-audit
```

---

## 💬 Usage Triggers

You can activate this skill by giving prompts such as:
- *"Check my server folder `/var/www/my-app` over SFTP."*
- *"Ye server ki directory check karke ek ek file ka kaam batao."*
- *"Audit remote directory on 192.168.1.100 and summarize what each script does."*
- *"Is remote folder me konsi file kya karti hai batao."*

---

## 🔒 Security & Safety Principles

1. **Ephemeral Credentials**: SSH passwords and private keys are never written to disk, session logs, or output reports.
2. **Directory Boundary**: The audit stays confined within the provided target path and never follows symlinks pointing outside.
3. **No Unsolicited Modifications**: Even if asked to edit or delete files during an audit, the skill refuses mutation operations and preserves read-only integrity.

---

## 📄 License

MIT License. See [LICENSE](LICENSE) for details.
