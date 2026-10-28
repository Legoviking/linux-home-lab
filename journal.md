# Linux Home Lab — Lab Journal

**Goal:** Learn Linux from zero and build a documented home lab over six months: an Ubuntu server running PostgreSQL, a Docker-hosted app, monitoring, automated backups, SSH and a firewall, all written up here.

| Setup | |
|---|---|
| Host machine | Windows 11 desktop, 16 GB RAM |
| Linux environment | Ubuntu on WSL 2 (Windows Subsystem for Linux) |
| Budget | $0 |
| Pace | ~5–7 hours/week |

## Progress

| # | Date | Topic | Status |
|---|---|---|---|
| 1 | 2026-10-19 | Install Ubuntu (WSL 2) + navigating Linux | ✅ Done |
| 2 | 2026-10-19 | Creating, editing and reading files | ✅ Done |
| 3 | 2026-10-20 | Copying, moving, renaming and deleting | ✅ Done |
| — | 2026-10-20 / 10-26 | Review mini-lab (Lessons 1–3) | 🟡 Challenges 1–2 done |
| 4 | 2026-10-26 | Users, groups, permissions and `sudo` | ✅ Done |
| 4 | 2026-10-27 | Lesson 4 redo + password security briefing | ✅ Done |
| 5 | 2026-10-28 | Installing software (`apt`) + system info | ✅ Done |
| — | — | Week 1 mini-project: `homelab/` folder + system info report | ⬜ Next |

---

## Entry 1 — 2026-10-19 — Setup + Lesson 1: Navigating Linux

**Goal:** Get Ubuntu running on my Windows PC and learn to find my way around the filesystem.

**What I did**
- Installed Ubuntu on Windows 11 using WSL 2 and created my Linux user account.
- Ran my first commands and created my first directories from the terminal.

**Commands learned**

| Command | What it does |
|---|---|
| `whoami` | Shows which user I'm logged in as |
| `pwd` | Print working directory: shows where I am |
| `ls` | Lists files and directories |
| `ls -la` | Detailed list (`-l`) including hidden files (`-a`) |
| `cd <dir>` | Change directory |
| `cd ~` / `cd` | Go to my home directory |
| `cd ..` | Go up one directory |
| `cd /` | Go to the root of the filesystem |
| `mkdir <name>` | Make a directory |

**What tripped me up → how I fixed it**
- **The first setup instructions assumed too much.** I asked for step-by-step instructions on where to get Ubuntu and what WSL is, then completed the install.
- **`ls` printed nothing.** I thought it failed. It hadn't: the directory was just empty. *Lesson: a Linux command can succeed without printing any output.*

**Key concepts**
- Linux is technically the **kernel**. **Ubuntu** is a **distribution** that packages the kernel with tools.
- The **terminal** is the window, and the **shell** (Bash) interprets what I type.
- The prompt `user@hostname:~$` shows my user, my machine, my current directory (`~` = home) and that I'm a regular user (`$`).
- Options like `-l` and `-a` change how a command behaves. `man <command>` shows the manual.

---

## Entry 2 — 2026-10-19 — Lesson 2: Creating, Editing and Reading Files

**Goal:** Create a text file, write in it, and read it back, all from the terminal.

**What I did**
```bash
cd ~
mkdir lesson2
cd lesson2
touch notes.txt
nano notes.txt      # typed three lines, saved, exited
cat notes.txt       # displayed the three lines
```

**Commands learned**

| Command | What it does |
|---|---|
| `touch <file>` | Creates an empty file (or updates its timestamp) |
| `nano <file>` | Opens a simple terminal text editor |
| `cat <file>` | Displays a file's contents |

**nano basics:** `Ctrl+O` then `Enter` saves. `Ctrl+X` exits. (`^` in nano's menu means Ctrl.)

**Key concepts**
- **Absolute path:** starts from root, e.g. `/home/<username>/lesson2/notes.txt`.
- **Relative path:** starts from where I am now, e.g. `notes.txt` when I'm already in `lesson2`.
- Linux doesn't need file extensions. `.txt` is just there to make the purpose obvious.
- Servers keep most settings, logs and configs in text files, which is why reading and editing them from the terminal matters.

**Other notes from this session**
- Running Ubuntu in WSL doesn't interfere with normal Windows apps. Files stay saved after closing the terminal; type `exit` to close cleanly.

---

## Entry 3 — 2026-10-20 — Lesson 3: Copying, Moving, Renaming and Deleting

**Goal:** Manage files the way an admin would, e.g. backing up a file before changing it.

**What I did**
```bash
cd ~/lesson2
cp notes.txt backup.txt            # copy
mv backup.txt old-notes.txt        # rename
mkdir backups
mv old-notes.txt backups/          # move into a directory
cat backups/old-notes.txt          # read a file without entering its directory
touch delete-me.txt
rm -i delete-me.txt                # delete with confirmation
mkdir temporary
rmdir temporary                    # remove an empty directory
```

**Final challenge: organize files**
```bash
mkdir projects
cp notes.txt projects/
ls projects/
mv projects/notes.txt projects/project-notes.txt
cat projects/project-notes.txt
mkdir testfolder
rmdir testfolder
```

**Commands learned**

| Command | What it does |
|---|---|
| `cp <source> <destination>` | Copies a file (original stays) |
| `mv <source> <destination>` | Moves **or** renames a file |
| `rm <file>` | Deletes a file. **No Recycle Bin.** |
| `rm -i <file>` | Deletes only after asking for confirmation |
| `rmdir <dir>` | Removes a directory, only if it's empty |
| `ls <dir>/` | Lists another directory without entering it |
| `man <command>` | Opens the manual for a command (`q` to quit) |

**What tripped me up → how I fixed it**
- **"Missing" `notes.txt`.** I couldn't see the file from the day before. I checked with `pwd`, then `cd ~/lesson2` and `ls -la`, and it was there. *Lesson: confirm where I am before assuming a file is gone.*
- **`cat notes` didn't work.** Linux needs the exact name and path: `cat backups/old-notes.txt`. It won't guess the extension or search other folders.
- **`cp notes.txt projects.txt`** creates a new *file* called `projects.txt`. To copy *into* the directory I needed `cp notes.txt projects/`.
- **`mv notes.txt projects-notes.txt`** would rename the *original*, not the copy. To rename the copy, both paths need the folder: `mv projects/notes.txt projects/project-notes.txt`.
- **`mkdir test folder`** creates **two** directories (`test` and `folder`), because spaces separate arguments. Use `testfolder` (or `test-folder`).
- **`rm testfodler`** had a typo and used the wrong command. For an empty directory it's `rmdir testfolder`. *Tip: press Tab to auto-complete names and avoid typos.*
- **Why `rmdir backups` fails:** `backups` still contained `old-notes.txt`, and `rmdir` only removes empty directories. That's a built-in safety net.

**Key concepts**
- Commands read **left to right: command → source → destination**.
- A trailing `/` signals I'm pointing at a directory.
- Linux is literal: one wrong letter or an extra space changes the result.
- Habit: **back up a file (`cp`) before editing it.**

---

## Entry 4 — 2026-10-20 — Review Mini-Lab (Lessons 1–3)

Doing the review without notes. The instructor checks my answers.

| Challenge | My answer | Result |
|---|---|---|
| 1. Go home, show full path, list contents | `cd ~` → `pwd` → `ls -la` | ✅ 3/3 (bonus for `-la`) |
| 2. Create `linux-review/`, enter it, create `practice.txt` | `mkdir linux-review` → `cd linux-review` → `touch practice.txt` (needed a reminder for `cd`) | ✅ |
| 3–5 | | ⬜ |

---

## Entry 5 — 2026-10-26 — Lesson 4: Users, Groups, Permissions and `sudo`

**Goal:** Understand who can do what to a file, change it, and use admin power safely.

**What I did**
```bash
whoami; id; groups                  # I'm ed (uid 1000), and I'm in the sudo group
sudo groupadd homelab               # created a shared group
sudo chgrp homelab practice.txt     # file now owned by ed, group homelab
sudo usermod -aG homelab ed         # joined the group (showed up after reopening Ubuntu)
touch secret.txt
chmod o-r secret.txt                # others can't read
chmod g-r secret.txt                # group can't read  →  -rw------- (600)
nano hello.sh                       # first script: #!/bin/bash + echo
./hello.sh                          # Permission denied (no x)
chmod u+x hello.sh                  # added execute for owner → script ran
chmod 755 hello.sh                  # rwxr-xr-x using the number shortcut
sudo whoami                         # root — sudo runs ONE command as root
sudo touch rootfile.txt             # owned by root → I couldn't write to it
sudo chown ed rootfile.txt          # took ownership back → echo >> worked
```

**Commands learned**

| Command | What it does |
|---|---|
| `id`, `groups` | Show my user ID and the groups I'm in |
| `ls -l` (letter L, "long") | Show permissions, owner, group, size, date |
| `chmod u/g/o +/- r/w/x <file>` | Add or remove a permission for owner / group / others |
| `chmod 755 <file>` | Set all permissions at once with numbers |
| `chown <user> <file>` | Change a file's owner (needs `sudo`) |
| `chgrp <group> <file>` | Change a file's group (needs `sudo`) |
| `groupadd <name>` | Create a group (needs `sudo`) |
| `usermod -aG <group> <user>` | Add a user to a group, **keeping** their other groups |
| `sudo <command>` | Run one command as root |
| `passwd` | Change my password |
| `nano <file>` | Edit a file in the terminal (Ctrl+O save, Ctrl+X exit) |
| `./script.sh` | Run a script in the current folder |
| `<command> --help` / `man <command>` | Quick summary / full manual (`/` to search, `q` to quit) |

**What tripped me up → how I fixed it**
- **Forgot the command to enter a folder.** It's `cd` = **c**hange **d**irectory. Checked with `pwd` first, which was the right instinct.
- **`ls -1` vs `ls -l`.** I was typing the number **1** (names only) instead of the letter **l** (long listing). They look the same in the terminal font. *Lesson: if the output isn't what I expected, check exactly what I typed.*
- **`ls-la` needs a space:** `ls -la`. Options always come after a space.
- **`chmod g -r` didn't work.** No space inside `g-r`, and the command needs a filename: `chmod g-r secret.txt`.
- **Didn't remember setting a `sudo` password.** It was created during the Ubuntu install. Reset it from Windows PowerShell with `wsl -u root`, then `passwd ed`. *Lesson: on WSL, whoever controls the Windows account can become root, so account access = admin access.*
- **Couldn't find `-aG` in the man page.** Searches are case-sensitive (`/-G` ≠ `/-g`), and WSL man pages can be thin. `usermod --help | grep ...` found it faster.
- **`./hello.sh` → Permission denied.** The file had no `x`. Fixed with `chmod u+x hello.sh`.
- **Couldn't write to `rootfile.txt`.** `sudo` made root the owner, so I was "others" (`r--`). Fixed with `sudo chown ed rootfile.txt`.

**Key concepts**
- `-rwxr-xr-x` = type + **owner** + **group** + **others**, 3 letters each.
- **r** read, **w** write, **x** execute. On a **folder**, `x` = allowed to *enter* it (`cd`), `r` = list it, `w` = create/delete inside it.
- **Numbers:** r=4, w=2, x=1. Add each chunk, always 3 digits: `755` scripts/folders, `644` normal files, `600` private files.
- `.` = this folder, `..` = one folder up. `./hello.sh` = "the file right here." Bash only searches the **PATH** for commands.
- `#!/bin/bash` (the shebang) on line 1 tells Linux which program runs the script. `.sh` is just a label.
- Ubuntu gives every user a private group with the same name (`ed ed`). That's normal; make **new** groups for shared access instead of renaming it.
- `usermod -G` alone **replaces** all my groups, which would remove `sudo`. Always `-aG` (**a**ppend).

**`sudo` safety rules**
1. Never `sudo` a command I don't understand.
2. `ls` before any `sudo rm`.
3. Never `chmod 777`.
4. "Permission denied" → check `ls -l` first, then fix that specific permission or owner.

**Knowledge check:** 3 correct, 2 partial. Review: the three chunks are **owner / group / others**, and `x` on a folder means *enter*.

---

## Entry 6 — 2026-10-27 — Lesson 4 Redo: Permissions + Password Security

**Goal:** Fix my two weak spots from Entry 5 (the three permission classes, and `x` on a folder), then learn how Linux handles passwords.

**What I did**
```bash
mkdir -p lesson4 && cd lesson4      # && = run the 2nd command only if the 1st works
touch notes.txt
chmod 640 notes.txt                 # owner rw, group r, others nothing (numeric)
chmod u=rw,g=r,o= notes.txt         # same result (symbolic, using = to set exactly)
./hello.sh                          # Permission denied → chmod u+x hello.sh
mkdir tabtest && cd tabtest         # practiced Tab completion (apples / apricots / bananas)
cd ..                               # up one folder;  cd -  = back to the previous folder
sudo chage -l $USER                 # password age info for my account
sudo -k                             # end the sudo timer early
```

**Commands learned**

| Command | What it does |
|---|---|
| `cmd1 && cmd2` | Runs `cmd2` only if `cmd1` succeeded |
| `# comment` | Everything after `#` is ignored by the shell |
| `cd -` | Go back to the previous directory |
| `chmod u=rw,g=r,o= <file>` | Set permissions **exactly** (`=`), instead of adding (`+`) or removing (`-`) |
| `chown <user>:<group> <file>` | Change owner and group in one command |
| `sudo -k` / `sudo -l` | End the sudo timer / list what I'm allowed to run with sudo |
| `sudo chage -l <user>` | Show password age and expiry info |
| `Ctrl +` / `Ctrl −` / `Ctrl 0` | Make the terminal font bigger / smaller / reset |

**What tripped me up → how I fixed it**
- **Typo: `mkdir -p lesson && cd lesson4`** created the wrong folder. Fixed with `cd ~`, `rmdir lesson`, then `mkdir -p lesson4 && cd lesson4`. *Lesson: read the whole command before pressing Enter.*
- **"No such file or directory" on `chmod g-r notes.txt`.** I was in the wrong folder and the file didn't exist yet. Checked `pwd` and `ls`, then `cd ~/lesson4` and `touch notes.txt`.
- **Wrote `g+r` when I meant "group gets exactly read."** `+` only adds to what's already there. `=` sets it exactly.
- **First said `x` on a folder means "execute the file."** After the vault lab: `x` on a folder means I can **enter** it and reach the files inside.
- **Knew `usermod -G` needed `-a` but not why.** `-G` alone **replaces** all my groups, so it can kick me out of `sudo`.

**Key concepts**
- Linux stores password **hashes**, not passwords. `/etc/passwd` lists users (anyone can read it). `/etc/shadow` holds the hashes (root only). Weak passwords can still be cracked offline from a stolen hash.
- Ubuntu locks the root account. Admin access goes through `sudo`, which stays unlocked for about 15 minutes.
- Forgot my WSL password → in PowerShell: `wsl -u root`, then `passwd ed`, then `exit`.
- **Length beats complexity:** a 4–5 word passphrase (16+ characters). Never reuse passwords (credential stuffing). Use a password manager and MFA (an authenticator app).
- **Never commit secrets to this public repo.** If one leaks, change the password first, then clean up.

**Knowledge check:** Permission challenges 5/5 (both weak spots cleared). Password field check 3.5/4. Review: credential stuffing.

---

## Entry 7 — 2026-10-28 — Lesson 5: Installing Software (`apt`) + System Info

**Goal:** Install, update and remove software the Linux way, and find out what hardware my lab runs on.

**What I did**
```bash
sudo apt update                     # refresh the package list (installs nothing)
sudo apt upgrade                    # install newer versions of what I already have
sudo apt install cowsay
cowsay "lab online"
sudo apt purge cowsay               # remove the program AND its config files
sudo apt autoremove                 # remove leftover dependencies nothing needs
apt install tree                    # no sudo → Permission denied (on purpose)
free -h                             # RAM
lscpu                               # CPU details
nproc                               # number of CPU threads

# remove vs purge lab
sudo apt remove screen
ls -l /etc/screenrc                 # config file still there!
dpkg -l screen                      # status rc = removed, config remains
sudo apt purge screen
ls -l /etc/screenrc                 # No such file or directory
sudo apt install ubuntu-release-upgrader-core update-manager-core   # put back what remove took
```

**Commands learned**

| Command | What it does |
|---|---|
| `sudo apt update` | Refreshes the list of available packages. Run it **first** |
| `sudo apt upgrade` | Installs newer versions of installed packages |
| `apt list --upgradable` | Previews what would be upgraded |
| `apt search <name>` / `apt show <pkg>` | Finds a package / shows its details |
| `sudo apt install <pkg>` | Installs a package (and its dependencies) |
| `sudo apt remove <pkg>` | Removes the program, **keeps** its config files |
| `sudo apt purge <pkg>` | Removes the program **and** its config files |
| `sudo apt autoremove` | Removes dependencies that nothing needs anymore |
| `dpkg -l <pkg>` | Package status: `ii` = installed, `rc` = removed but config left |
| `which <cmd>` | Shows where a command lives |
| `free -h` | RAM in human-readable units (look at `total` and `available`) |
| `nproc` / `lscpu` | Number of CPU threads / full CPU details |
| `cat /etc/os-release`, `uname -r`, `hostname`, `uptime`, `df -h` | OS version, kernel, machine name, time running, disk space |

**What tripped me up → how I fixed it**
- **Tried `rmdir cowsay` to uninstall.** `rmdir` is only for empty folders. *Lesson: if apt installed it, apt removes it.*
- **Forgot `purge` and `autoremove`.** Re-learned them with a hands-on lab.
- **Typed `""still here?"` (3 quotes).** The shell sat at a `>` prompt waiting for the closing quote. **Ctrl + C** cancels.
- **Stuck at `lines 1-6/6 (END)`.** That's a pager (a viewer). Press **`q`** to quit.
- **Removing `screen` also removed two system packages** that depend on it. Reinstalling them brought `screen` back as a dependency. *Lesson: always read the `REMOVING:` list before pressing Y, and don't run `autoremove` blindly.*
- **Thought `remove` was best before a clean reinstall.** Wrong. `remove` keeps the old (maybe broken) config, so the problem comes back. Use **`purge`** for a fresh start.

**Key concepts**
- `apt install` needs `sudo` because it **writes** to root-owned system folders. `apt search` only **reads**, so it doesn't.
- **remove** = program goes. **purge** = program + settings go. **autoremove** = unused leftovers go.
- **remove** when I'll reinstall and want my settings back. **purge** when something's broken or I want it gone for good.
- A `[Y/n]` prompt: the capital letter is the default, so Enter = yes.
- My lab: **Intel Core i7-13700KF** (16 cores / 24 threads, so `nproc` = 24), **~15 GiB RAM** visible to Ubuntu, **Ubuntu 26.04.1 LTS** on WSL 2.
- Linux counts **threads** as CPUs. `lscpu` inside WSL shows a simplified virtual layout (12 cores × 2), not the real chip.

**Knowledge check:** 4/4 challenges. Missed `purge`/`autoremove` on recall, then cleared both in the review lab.

---

## Command Cheat Sheet (Lessons 1–5)

| Category | Commands |
|---|---|
| Where am I / what's here | `pwd`, `ls`, `ls -la`, `whoami` |
| Moving around | `cd <dir>`, `cd ~`, `cd ..`, `cd -`, `cd /` |
| Creating | `mkdir`, `touch`, `nano` |
| Reading | `cat` |
| Copy / move / rename | `cp`, `mv` |
| Deleting | `rm -i`, `rmdir` |
| Users and permissions | `id`, `groups`, `chmod`, `chown`, `chgrp`, `usermod -aG`, `sudo` |
| Software | `apt update`, `apt upgrade`, `apt install`, `apt remove`, `apt purge`, `apt autoremove`, `dpkg -l` |
| System info | `free -h`, `nproc`, `lscpu`, `df -h`, `uname -r`, `uptime`, `cat /etc/os-release` |
| Help / escape | `man <command>`, `--help`, Tab completion, `q` (quit a pager), `Ctrl + C` (cancel) |

## Up Next
- Finish the review mini-lab (Challenges 3–5).
- **Week 1 mini-project:** build an organized `homelab/` folder and a system info report (`uname -a`, `free -h`, `df -h`), then push it to GitHub from the terminal.
- Set up GitHub sign-in from Ubuntu so I can `git push` from my lab.

---

### Template for future entries
```markdown
## Entry N — YYYY-MM-DD — Lesson X: <Topic>
**Goal:**
**What I did:** (commands in a code block)
**Commands learned:** (table)
**What tripped me up → how I fixed it:**
**Key concepts:**
```
