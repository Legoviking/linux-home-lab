# Linux Home Lab

I'm learning Linux from scratch and building a documented home lab over six months. Every session is logged in [journal.md](journal.md), including what broke and how I fixed it.

## End goal

A small, secure, self-hosted home lab built the way a professional would build it:

| Layer | What I'll run | Why it matters |
|---|---|---|
| Platform | **Proxmox VE** on a mini PC: containers (LXC) plus one Debian VM as the Docker host | Snapshots and isolation, so mistakes take minutes to undo |
| Network | Written network plan: subnets, static IPs/DHCP reservations, **VLANs** if the router supports them | Separating management, servers, IoT and guests from the start |
| DNS | **AdGuard Home**: network-wide ad blocking plus local names (`service.home.example.com`) | Services get names instead of IP:port |
| Front door | **Traefik** reverse proxy with real HTTPS certificates (Let's Encrypt DNS-01), private by default | One place for TLS, logins and access rules |
| Remote access | **Tailscale**: no ports opened to the internet | Avoids the most common beginner mistake |
| Secrets | Password manager, one item per service; nothing in plain text in configs | Easy to rotate and never committed to Git |
| Backups | **3-2-1**: Proxmox backups plus restic/Backrest to off-site storage, with a **tested restore** | A backup isn't real until it's been restored |
| Monitoring | **Uptime Kuma** status page, **ntfy** phone alerts, **Homepage** dashboard | Know when something breaks before anyone else does |
| Capstone app | **PostgreSQL** with sample business data plus a small app behind Traefik | Ties the lab to my SQL and implementation background |

Every config, script, network diagram and "why I set it up this way" note will live in this repo.

## Roadmap

| Month | Focus | Milestone |
|---|---|---|
| 1 | Linux fundamentals on WSL: terminal, files, permissions, packages, processes | Comfortable on the command line |
| 2 | Networking (IP, subnets, DNS, ports), SSH keys, hardening; get a mini PC and install **Proxmox**; write the network plan | First real server running |
| 3 | Docker on a Debian VM; **AdGuard Home**, **Traefik** with HTTPS, **Homepage** | Core stack live with real certificates |
| 4 | **Tailscale**, VLANs/firewall rules, password-manager secrets; Bash scripting | Secure remote access, no open ports |
| 5 | **Backups (3-2-1) with a restore test**, **Uptime Kuma** + **ntfy**, Git workflow | Lab survives a failure and alerts me |
| 6 | Capstone: PostgreSQL app behind Traefik, full documentation, interview walkthrough | Finished, documented home lab |

## Progress

| # | Topic | Status |
|---|---|---|
| 1 | Installing Ubuntu (WSL 2) + navigating the filesystem | ✅ |
| 2 | Creating, editing and reading files (`touch`, `nano`, `cat`) | ✅ |
| 3 | Copying, moving, renaming, deleting (`cp`, `mv`, `rm`, `rmdir`) | ✅ |
| 4 | Users, groups, permissions and `sudo` (`chmod`, `chown`, `usermod`) | ✅ |
| 5 | Installing software (`apt`) + system info | ⬜ Next |

## Setup

| | Now | Month 2+ |
|---|---|---|
| Host | Windows 11 desktop, 16 GB RAM | Used mini PC running Proxmox VE |
| Linux | Ubuntu on WSL 2 | Debian/Ubuntu VMs and LXC containers |
| Budget | $0 | ~$100–200 for a used mini PC; optional domain (~$10/yr) |

## Repo layout

- `README.md`: this page
- `journal.md`: dated lab journal (commands, mistakes, fixes, key concepts)
- `scripts/`: shell scripts as I write them (coming soon)
- `docs/`: network plan and design decisions (coming Month 2)
