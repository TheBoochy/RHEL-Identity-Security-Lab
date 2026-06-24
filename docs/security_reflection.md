## SELinux review

SELinux was enabled and running in enforcing mode.

The loaded SELinux policy was `targeted`.

The shared directories under `/srv/company` used the context:

```text
unconfined_u:object_r:var_t:s0
```

This shows that Linux permissions and SELinux labels exist as separate security layers.

Normal Linux permissions control access based on users, groups and mode bits. SELinux adds policy-based access control through security contexts.

No SELinux policy changes were made in this lab. The SELinux part was used as a baseline review and documentation step.

---

## Firewalld review

Firewalld was running and using the public zone on interface `ens160`.

The allowed services were:

* cockpit
* dhcpv6-client
* ssh

No custom ports were open before testing.

A temporary test opened TCP port 8080, verified that it appeared in the open port list, then removed it again.

No permanent firewall changes were made.

This demonstrates safe firewall review and temporary rule testing without leaving unnecessary open ports behind.

---

## Password and account policy

The lab reviewed password aging settings for `rheladmin` and `restricteduser`.

A password policy was applied to `rheladmin`:

| Setting              | Value   |
| -------------------- | ------- |
| Minimum password age | 7 days  |
| Maximum password age | 90 days |
| Warning period       | 14 days |

The `restricteduser` account was reviewed but not changed.

The lab also checked failed login records with `faillock`.

This demonstrates basic account policy review, password aging configuration and failed login inspection.

---

## Audit and log review

The lab reviewed logs using:

* `journalctl`
* `last`
* `who`
* `faillock`
* `ausearch`

The review included sudo activity, SSH service logs, recent login history, currently logged-in users and failed login records.

The `ausearch` command returned no recent `USER_LOGIN` matches, which was documented.

This demonstrates basic security operations log review and shows how Linux systems can be checked for authentication and privilege-related events.

---

## Security principles demonstrated

This lab demonstrated several important security principles:

| Principle                       | How it was demonstrated                                                    |
| ------------------------------- | -------------------------------------------------------------------------- |
| Least privilege                 | Users only received access needed for their role.                          |
| Separation of duties            | Admin, developer, audit and restricted roles were separated.               |
| Controlled privilege escalation | Only `rheladmin` received sudo access.                                     |
| Group-based access control      | Shared directories were assigned to role-based groups.                     |
| Defense in depth                | Linux permissions, SELinux and firewalld were reviewed as separate layers. |
| Account lifecycle control       | Password aging and account status were reviewed.                           |
| Auditability                    | Commands, screenshots and logbook entries documented each step.            |

---

## Limitations

The lab had some limitations:

* The system was not registered with Red Hat subscription management.
* No repositories were enabled.
* The ACL tools could not be installed.
* ACL permissions could not be configured.
* SELinux policy changes were not tested.
* The lab used local test users instead of a central identity system.
* The environment was a virtual lab, not a production server.

These limitations were documented instead of ignored.

---

## Future improvements

Possible future improvements include:

* Registering the RHEL system or enabling valid repositories.
* Installing ACL tools and completing ACL permission testing.
* Testing SELinux context changes with `restorecon` and `semanage fcontext`.
* Adding more detailed sudoers rules for limited audit access.
* Adding automated scripts for user and group verification.
* Exporting selected command outputs into the `results` folder.
* Expanding log review with more auditd examples.
* Adding a final architecture or access-control diagram.

---

## Conclusion

The RHEL Identity and Security Operations Lab demonstrated practical Linux security administration tasks.

The lab covered identity management, sudo policy, group-based filesystem permissions, ACL tooling checks, SELinux review, firewalld review, password policy and log review.

The work shows how a Linux system can be configured, verified and documented using a structured security operations approach.
