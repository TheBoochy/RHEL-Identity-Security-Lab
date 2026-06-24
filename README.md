# RHEL Identity and Security Operations Lab

## Overview

RHEL Identity and Security Operations Lab is a voluntary sysadmin portfolio project focused on Red Hat Enterprise Linux identity management, permissions, access control and security operations.

The lab demonstrates practical Linux administration tasks using local users, groups, sudo policy, shared directory permissions, SELinux review, firewalld review, password/account policy and log review.

Public documentation uses the name **Vulkan**.

---

## Scenario

A small organization needs a Red Hat Enterprise Linux server with controlled user access and documented security settings.

The server is used to practice identity and access management tasks such as creating users, assigning groups, controlling sudo access, configuring shared directories, reviewing SELinux status, reviewing firewall configuration, applying account policy and reviewing security logs.

The lab is not a production deployment. It is a controlled virtual lab environment created for learning, documentation and portfolio evidence.

---

## Environment

| Item                   | Detail                        |
| ---------------------- | ----------------------------- |
| Operating system       | Red Hat Enterprise Linux 10.1 |
| Version name           | Coughlan                      |
| Hostname               | srv-linuxops01                |
| Virtualization         | VMware                        |
| Network interface      | ens160                        |
| IP address             | 192.168.80.134/24             |
| SELinux mode           | Enforcing                     |
| Firewall service       | firewalld                     |
| Documentation identity | Vulkan                        |

---

## Project structure

```text
RHEL-Identity-Security-Lab/
├── docs/
│   └── security_reflection.md
├── results/
│   └── .gitkeep
├── screenshots/
│   ├── screenshot-01-project-structure-and-git-status.png
│   ├── screenshot-02a-rhel-baseline-system.png
│   ├── screenshot-02b-rhel-baseline-security-and-resources.png
│   ├── screenshot-03-rhel-users-and-groups-created.png
│   ├── screenshot-04-rhel-sudo-policy.png
│   ├── screenshot-05-rhel-shared-directory-permissions.png
│   ├── screenshot-06a-rhel-acl-tools-unavailable.png
│   ├── screenshot-06b-rhel-repository-status.png
│   ├── screenshot-07-rhel-selinux-basics.png
│   ├── screenshot-08-rhel-firewalld-review.png
│   ├── screenshot-09-rhel-password-account-policy.png
│   ├── screenshot-10a-rhel-sudo-ssh-logs.png
│   ├── screenshot-10b-rhel-login-audit-review.png
│   └── screenshot-11-security-reflection-document.png
├── scripts/
│   └── .gitkeep
├── logbook.md
└── README.md
```

---

## Completed lab parts

| Part    | Topic                                 | Status   |
| ------- | ------------------------------------- | -------- |
| Part 1  | Repository setup and planning         | Complete |
| Part 2  | RHEL server baseline verification     | Complete |
| Part 3  | Users and groups                      | Complete |
| Part 4  | Sudo policy                           | Complete |
| Part 5  | Shared directory permissions          | Complete |
| Part 6  | ACL tooling and repository limitation | Complete |
| Part 7  | SELinux basics                        | Complete |
| Part 8  | Firewalld review                      | Complete |
| Part 9  | Password and account policy           | Complete |
| Part 10 | Audit and log review                  | Complete |
| Part 11 | Security reflection                   | Complete |
| Part 12 | Final README and GitHub polish        | Complete |

---

## Users and groups

The lab created role-based groups and users.

| Group      | Purpose                  |
| ---------- | ------------------------ |
| rheladmins | Administrator role group |
| developers | Developer role group     |
| auditors   | Audit/review role group  |
| restricted | Restricted role group    |

| User           | Purpose              |
| -------------- | -------------------- |
| rheladmin      | Admin test user      |
| devuser1       | Developer test user  |
| devuser2       | Developer test user  |
| audituser      | Audit test user      |
| restricteduser | Restricted test user |

This structure demonstrates role-based access control and supports the principle of least privilege.

---

## Sudo policy

The `rheladmin` account was added to the `wheel` group and verified with:

```bash
sudo whoami
```

The command returned:

```text
root
```

This confirmed that the admin test account could use sudo privileges.

The `restricteduser` account was also tested with `sudo whoami` and was denied access. This confirmed that restricted users did not automatically receive administrator privileges.

---

## Shared directory permissions

The lab created shared directories under `/srv/company`.

| Directory                  | Group      |
| -------------------------- | ---------- |
| `/srv/company/development` | developers |
| `/srv/company/audit`       | auditors   |
| `/srv/company/restricted`  | restricted |

The directories were configured with:

```bash
chmod 2770
```

This enabled group-based access and setgid inheritance.

Access testing confirmed that:

* `devuser1` could create files in the development directory.
* `devuser1` could not access the audit directory.
* `audituser` could create files in the audit directory.
* `audituser` could not access the development directory.

This demonstrates controlled shared storage using Linux ownership, group permissions and directory mode settings.

---

## ACL tooling limitation

ACL configuration was planned, but the required tools were not installed:

```bash
setfacl
getfacl
```

The `acl` package was also not installed.

An installation attempt using `dnf` failed because the RHEL system was not registered with a Red Hat entitlement server and had no enabled repositories.

This limitation was documented as a realistic system administration issue.

Future remediation would require registering the system with Red Hat subscription management or enabling a valid repository source before installing the missing ACL tools.

---

## SELinux review

SELinux was reviewed and confirmed to be enabled and enforcing.

The loaded policy was:

```text
targeted
```

The shared directories under `/srv/company` used the context:

```text
unconfined_u:object_r:var_t:s0
```

No SELinux policy changes were made in this lab.

This part demonstrated how SELinux labels exist alongside normal Linux permissions as an additional security layer.

---

## Firewalld review

Firewalld was running and using the `public` zone on interface `ens160`.

Allowed services:

* cockpit
* dhcpv6-client
* ssh

No custom ports were open before testing.

A temporary test opened TCP port `8080`, verified that it appeared in the open port list, and removed it again.

No permanent firewall changes were made.

This demonstrated safe firewall review and temporary firewall rule testing.

---

## Password and account policy

Password aging settings were reviewed for `rheladmin` and `restricteduser`.

A password aging policy was applied to `rheladmin`.

| Setting              | Value   |
| -------------------- | ------- |
| Minimum password age | 7 days  |
| Maximum password age | 90 days |
| Warning period       | 14 days |

The `restricteduser` account was reviewed but not changed.

Failed login records were checked with:

```bash
faillock
```

This demonstrated basic account policy review, password aging configuration and failed login inspection.

---

## Audit and log review

The lab reviewed system and authentication-related information using:

* `journalctl`
* `last`
* `who`
* `faillock`
* `ausearch`

The review included sudo activity, SSH service logs, login history, active users and failed login records.

The `ausearch` command returned no recent `USER_LOGIN` matches, which was documented.

This demonstrated basic security operations log review.

---

## Security reflection

A separate reflection document was created:

```text
docs/security_reflection.md
```

This document summarizes the security lessons from the lab, including:

* identity and access control
* least privilege
* sudo access
* shared directory permissions
* ACL limitations
* SELinux review
* firewalld review
* password and account policy
* audit and log review
* limitations and future improvements

---

## Skills demonstrated

This project demonstrates:

* Red Hat Enterprise Linux administration
* Linux users and groups
* Role-based access control
* sudo privilege management
* Linux filesystem permissions
* setgid directory inheritance
* SELinux status and context review
* firewalld firewall review
* temporary firewall port testing
* password aging policy
* failed login review
* system log review
* troubleshooting missing tools and repository limitations
* Git and GitHub documentation workflow
* Markdown documentation
* screenshot-based evidence collection

---

## Documentation and evidence

Main documentation files:

| File                          | Purpose                                 |
| ----------------------------- | --------------------------------------- |
| `README.md`                   | Project overview and final summary      |
| `logbook.md`                  | Step-by-step project documentation      |
| `docs/security_reflection.md` | Security reflection and lessons learned |

Screenshot evidence is stored in:

```text
screenshots/
```

---

## Limitations

The lab had several limitations:

* The RHEL system was not registered with Red Hat subscription management.
* No repositories were enabled.
* ACL tools could not be installed.
* ACL permission configuration could not be completed.
* SELinux policy changes were not tested.
* The lab used local users instead of centralized identity management.
* The environment was a virtual lab, not a production system.

These limitations were documented instead of ignored.

---

## Final status

The RHEL Identity and Security Operations Lab is complete.

The project demonstrates practical Linux identity and security administration using a structured lab workflow with command verification, screenshots, documentation and GitHub version control.
