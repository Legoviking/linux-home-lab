# Linux Home Lab

I'm learning Linux from scratch and building a documented home lab over six months. Every session is logged in [journal.md](journal.md), including what broke and how I fixed it.

## Setup

| | |
|---|---|
| Host | Windows 11 desktop, 16 GB RAM |
| Linux | Ubuntu on WSL 2 (Windows Subsystem for Linux) |
| Budget | $0 to start |
| Pace | ~5–7 hours/week |

## Progress

| # | Topic | Status |
|---|---|---|
| 1 | Installing Ubuntu (WSL 2) + navigating the filesystem | ✅ |
| 2 | Creating, editing and reading files (`touch`, `nano`, `cat`) | ✅ |
| 3 | Copying, moving, renaming, deleting (`cp`, `mv`, `rm`, `rmdir`) | ✅ |
| 4 | Users, groups, permissions and `sudo` (`chmod`, `chown`, `usermod`) | ✅ |
| 5 | Installing software (`apt`) + system info | ⬜ Next |

## Roadmap

| Month | Focus |
|---|---|
| 1 | Linux fundamentals: terminal, filesystem, permissions |
| 2 | Server administration: networking, SSH, troubleshooting |
| 3 | Docker, services, databases (PostgreSQL) |
| 4 | Bash scripting, automated backups, security |
| 5 | Monitoring, Git, multi-service setup |
| 6 | Capstone: documented, working home lab |

## End goal

A Linux server running:
- PostgreSQL with sample business data
- A Docker-hosted application
- A system health dashboard
- Automated backups
- SSH key access and a firewall

All configs, scripts and troubleshooting notes will live in this repo.

## Repo layout

- `README.md`: this page
- `journal.md`: dated lab journal (commands, mistakes, fixes, key concepts)
- `scripts/`: shell scripts as I write them (coming soon)
