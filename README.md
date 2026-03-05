# Laravel Artisan Command Automation via Ansible

This repository enables developers to securely request the execution of approved Laravel Artisan commands on the migration server via the DevOps automation pipeline (Semaphore or local Ansible).
---
##  Quick Start
1. **Edit the config file:**
	 - Open `main.yml` in the config repo.
	 - Add the Artisan command(s) you want to run (see formats below).
2. **Commit & push your changes.**
3. **Trigger the job** in Semaphore UI (or let the pipeline auto-trigger).

##  Command Formats
You may specify either a single command or a list of commands in `main.yml`:

**Single Command:**
```yaml
artisan_command: migrate --force
```

**Multiple Commands:**
artisan_command:
	- migrate --force
	- db:seed --class=UserSeeder
```
Commands are executed in the order listed.

---

##  Rules & Allowed Changes
- **Only modify:** The `artisan_command` variable in `main.yml`.
- **Do NOT:** Add new variables or change the file structure.
- **Allowed commands:** Only valid and pre-approved Laravel Artisan commands (enforced by automation).
- **Disallowed/invalid commands:** Will cause the job to fail with a clear error.
- **Multiple commands:** Will run sequentially on the target server.

---

##  Execution Flow
1. Developer updates `main.yml` with desired command(s).
2. Changes are pushed to the repository.
3. The Dev runs the automation pipeline.
4. Commands are validated and executed on the Laravel server.
5. All executions are logged and auditable.

---

##  Security & Best Practices
- **Never add dangerous commands** (e.g., `down`, `up`, `env`) to the allowed list.
- **Review all changes** to `allowed_artisan_commands` via code review.
- **If unsure, ask the DevOps/SRE team before running unfamiliar commands.**
