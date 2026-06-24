# RHEL Identity and Security Operations Lab — Logbook

## 2026-06-24 — Part 1: Repository setup and planning

### Goal

Start the RHEL Identity and Security Operations Lab by creating the local project structure, initial documentation files and Git repository.

### Work completed

* Created the local project folder.
* Created the main documentation folders:

  * docs
  * screenshots
  * scripts
  * results
* Created README.md.
* Created logbook.md.
* Created .gitkeep files so Git can track empty folders.
* Initialized the local Git repository.
* Prepared the project for the first commit.

### Project structure

```text
RHEL-Identity-Security-Lab/
├── docs/
│   └── .gitkeep
├── results/
│   └── .gitkeep
├── screenshots/
│   └── .gitkeep
├── scripts/
│   └── .gitkeep
├── logbook.md
└── README.md
```

### Commands used

```powershell
mkdir RHEL-Identity-Security-Lab
cd RHEL-Identity-Security-Lab
mkdir docs
mkdir screenshots
mkdir scripts
mkdir results
New-Item README.md
New-Item logbook.md
git init
New-Item docs\.gitkeep
New-Item results\.gitkeep
New-Item screenshots\.gitkeep
New-Item scripts\.gitkeep
git status
```

### Command purpose

| Command                          | Purpose                                                |
| -------------------------------- | ------------------------------------------------------ |
| mkdir RHEL-Identity-Security-Lab | Creates the main project folder.                       |
| cd RHEL-Identity-Security-Lab    | Moves into the project folder.                         |
| mkdir docs                       | Creates the documentation folder.                      |
| mkdir screenshots                | Creates the screenshot evidence folder.                |
| mkdir scripts                    | Creates the script storage folder.                     |
| mkdir results                    | Creates the command output and result storage folder.  |
| New-Item README.md               | Creates the main project README file.                  |
| New-Item logbook.md              | Creates the project logbook file.                      |
| git init                         | Initializes a local Git repository.                    |
| New-Item .gitkeep                | Creates placeholder files so Git tracks empty folders. |
| git status                       | Shows untracked files and repository status.           |

### Notes

PowerShell does not handle `mkdir docs screenshots scripts results` the same way as Linux Bash. The folders were created one at a time instead.

Git does not track empty folders by default, so `.gitkeep` files were added to docs, results, screenshots and scripts.

This part prepares the project structure and documentation base for the rest of the lab.

### Evidence

Screenshot:

![screenshot-01-project-structure-and-git-status.png](screenshots/screenshot-01-project-structure-and-git-status.png)

---

## 2026-06-24 — Part 2: RHEL server baseline verification

### Goal

Verify the current Red Hat Enterprise Linux server baseline before configuring users, sudo policy, permissions, ACLs, SELinux settings or firewall rules.

### Work completed

* Connected to the RHEL server through SSH.
* Verified the server hostname.
* Verified the currently logged-in user.
* Verified the installed operating system and version.
* Verified the kernel and virtualization platform.
* Reviewed the active network interface and IP address.
* Reviewed disk usage.
* Reviewed memory and swap usage.
* Verified SELinux mode.
* Verified firewalld service status.
* Saved baseline verification screenshots.

### Verification results

| Item                  | Result                             |
| --------------------- | ---------------------------------- |
| Hostname              | srv-linuxops01                     |
| Logged-in user        | vulkan                             |
| Operating system      | Red Hat Enterprise Linux 10.1      |
| Version name          | Coughlan                           |
| Kernel                | Linux 6.12.0-124.8.1.el10_1.x86_64 |
| Virtualization        | VMware                             |
| Network interface     | ens160                             |
| IP address            | 192.168.80.134/24                  |
| Root filesystem       | /dev/mapper/rhel-root              |
| Root filesystem size  | 37 GB                              |
| Root filesystem usage | 6%                                 |
| Memory                | 1.6 GiB                            |
| Swap                  | 2.0 GiB                            |
| SELinux mode          | Enforcing                          |
| Firewall service      | firewalld                          |
| Firewall status       | active running                     |
| Firewall boot state   | enabled                            |

### Commands used

```bash
hostnamectl
whoami
cat /etc/os-release
ip addr
df
df -h
free -h
getenforce
sudo systemctl status firewalld --no-pager
```

### Command purpose

| Command                                    | Purpose                                                                                |
| ------------------------------------------ | -------------------------------------------------------------------------------------- |
| hostnamectl                                | Shows hostname, operating system, kernel, architecture and virtualization information. |
| whoami                                     | Shows the currently logged-in user.                                                    |
| cat /etc/os-release                        | Displays the installed Linux distribution and version details.                         |
| ip addr                                    | Shows network interfaces, MAC addresses and IP addresses.                              |
| df                                         | Shows filesystem disk usage in block format.                                           |
| df -h                                      | Shows filesystem disk usage in human-readable format.                                  |
| free -h                                    | Shows memory and swap usage in human-readable format.                                  |
| getenforce                                 | Shows the current SELinux mode.                                                        |
| sudo systemctl status firewalld --no-pager | Shows whether the firewalld service is loaded, enabled and running.                    |

### Notes

The server baseline was verified before making identity, permission or security configuration changes.

The server is running Red Hat Enterprise Linux 10.1 on VMware with the hostname srv-linuxops01. The active network interface is ens160, and the server IP address is 192.168.80.134/24.

SELinux is running in Enforcing mode, which is important for this lab because later parts will include SELinux review and context checks.

Firewalld is active and enabled, which confirms that the system firewall is running before any future firewall review or rule testing.

The `df` command was run once before `df -h`. The `df -h` output is easier to read, but both commands show filesystem usage.

This part provides a clean baseline for the rest of the RHEL Identity and Security Operations Lab.

### Evidence

Screenshots:

![screenshot-02a-rhel-baseline-system.png](screenshots/screenshot-02a-rhel-baseline-system.png)

![screenshot-02b-rhel-baseline-security-and-resources.png](screenshots/screenshot-02b-rhel-baseline-security-and-resources.png)

---

## 2026-06-24 — Part 3: Users and groups

### Goal

Create role-based Linux users and groups for the RHEL Identity and Security Operations Lab.

This part documents basic identity management by creating separate users and groups for administration, development, auditing and restricted access.

### Work completed

* Created the role-based groups:

  * rheladmins
  * developers
  * auditors
  * restricted

* Created the lab users:

  * rheladmin
  * devuser1
  * devuser2
  * audituser
  * restricteduser

* Added each user to the correct role-based group.

* Verified user and group membership with `id`.

* Verified group database entries with `getent group`.

* Saved screenshot evidence of the final verification output.

### Verification results

| Item                      | Result                     |
| ------------------------- | -------------------------- |
| Admin group               | rheladmins                 |
| Developer group           | developers                 |
| Audit group               | auditors                   |
| Restricted group          | restricted                 |
| Admin user                | rheladmin                  |
| Developer users           | devuser1, devuser2         |
| Audit user                | audituser                  |
| Restricted user           | restricteduser             |
| rheladmin membership      | rheladmin, rheladmins      |
| devuser1 membership       | devuser1, developers       |
| devuser2 membership       | devuser2, developers       |
| audituser membership      | audituser, auditors        |
| restricteduser membership | restricteduser, restricted |

### Commands used

```bash
sudo groupadd rheladmins
sudo groupadd developers
sudo groupadd auditors
sudo groupadd restricted

sudo useradd -m -s /bin/bash rheladmin
sudo useradd -m -s /bin/bash devuser1
sudo useradd -m -s /bin/bash devuser2
sudo useradd -m -s /bin/bash audituser
sudo useradd -m -s /bin/bash restricteduser

sudo passwd rheladmin
sudo passwd devuser1
sudo passwd devuser2
sudo passwd audituser
sudo passwd restricteduser

sudo usermod -aG rheladmins rheladmin
sudo usermod -aG developers devuser1
sudo usermod -aG developers devuser2
sudo usermod -aG auditors audituser
sudo usermod -aG restricted restricteduser

id rheladmin
id devuser1
id devuser2
id audituser
id restricteduser

getent group rheladmins
getent group developers
getent group auditors
getent group restricted
```

### Command purpose

| Command                   | Purpose                                                                          |
| ------------------------- | -------------------------------------------------------------------------------- |
| `groupadd`                | Creates a new Linux group.                                                       |
| `useradd -m -s /bin/bash` | Creates a new user, creates a home directory and sets Bash as the login shell.   |
| `passwd`                  | Sets or changes a user password.                                                 |
| `usermod -aG`             | Adds an existing user to an additional group without removing other memberships. |
| `id`                      | Shows a user's UID, primary group and additional group memberships.              |
| `getent group`            | Checks group information from the system group database.                         |

### Notes

The `rheladmins` group was verified and already contained the `rheladmin` user.

The `restricted` group existed but did not yet contain `restricteduser` when first checked.

The `developers` and `auditors` groups had to be created before users could be added to them. After creating the missing groups, all users were added to the correct role-based groups.

The group ID numbers may differ between systems. The important result is that each user appears in the correct group.

This part creates the identity foundation for later lab parts, including sudo policy, shared directory permissions, ACL permissions and audit review.

### Evidence

Screenshot:

![screenshot-03-rhel-users-and-groups-created.png](screenshots/screenshot-03-rhel-users-and-groups-created.png)

## 2026-06-24 — Part 4: Sudo policy

### Goal

Configure and verify controlled sudo access for the RHEL Identity and Security Operations Lab.

This part documents basic privilege management by allowing the admin test user to use sudo while confirming that the restricted test user does not have administrator access.

### Work completed

* Added `rheladmin` to the `wheel` group.
* Verified that `rheladmin` is a member of both `rheladmins` and `wheel`.
* Verified the current members of the `wheel` group.
* Switched to the `rheladmin` account.
* Confirmed that `rheladmin` can run commands with sudo privileges.
* Switched to the `restricteduser` account.
* Confirmed that `restricteduser` cannot run commands with sudo privileges.
* Saved screenshot evidence of the sudo policy verification.

### Verification results

| Item                           | Result                          |
| ------------------------------ | ------------------------------- |
| Admin test user                | rheladmin                       |
| Admin role group               | rheladmins                      |
| Sudo access group              | wheel                           |
| rheladmin sudo access          | Confirmed                       |
| rheladmin `sudo whoami` result | root                            |
| Restricted test user           | restricteduser                  |
| restricteduser sudo access     | Denied                          |
| restricteduser sudo result     | User is not in the sudoers file |

### Commands used

```bash
sudo usermod -aG wheel rheladmin

id rheladmin
getent group wheel

su - rheladmin
whoami
sudo whoami
exit

su - restricteduser
whoami
sudo whoami
exit
```

### Command purpose

| Command                            | Purpose                                                                               |
| ---------------------------------- | ------------------------------------------------------------------------------------- |
| `sudo usermod -aG wheel rheladmin` | Adds `rheladmin` to the `wheel` group without removing existing group memberships.    |
| `id rheladmin`                     | Verifies the user ID, primary group and additional group memberships for `rheladmin`. |
| `getent group wheel`               | Shows the current members of the `wheel` group.                                       |
| `su - rheladmin`                   | Switches to the `rheladmin` account and loads that user’s login environment.          |
| `whoami`                           | Shows the currently active user account.                                              |
| `sudo whoami`                      | Tests whether the current user can run a command with root privileges.                |
| `exit`                             | Logs out from the switched user session and returns to the previous user.             |
| `su - restricteduser`              | Switches to the restricted test account.                                              |

### Notes

On RHEL systems, the `wheel` group is commonly used to grant sudo privileges.

The `rheladmin` account successfully ran `sudo whoami`, and the command returned `root`. This confirms that the admin test account has working sudo access.

The `restricteduser` account attempted to run `sudo whoami`, but the system denied access and reported that the user is not in the sudoers file. This confirms that the restricted account does not have administrator privileges.

The “Last login” message appeared when switching users. This is normal and does not affect the test result.

This part creates the sudo access foundation for later security testing and permission reviews.

### Evidence

Screenshot:

![screenshot-04-rhel-sudo-policy.png](screenshots/screenshot-04-rhel-sudo-policy.png)
