# City-of-Bloomington.winbind

Install winbind, and join a linux host to an Active Directory domain

## Dependencies

City-of-Bloomington.linux

# Requirements

This role does not cover the Windows Active Directory setup required.
You must have already created the groups used for this server in Active Directory.

You need to create a keytab file with the credentials for the Windows admin user
that has permission to join computers to the domain.


# Role Variables

Available variables with example values

```yml
winbind_domain: ATHENA.MIT.EDU
winbind_workgroup: ATHENA

# If you need to lookup a group SID the easiest way to do so is via the command
# line: wbinfo -n <group name>
# This group will be granted SUDO access
winbind_group_admins: S-1-5-21-1004336348-1177238915-682003330-512
# This group will be generic users on the server
winbind_group_users: S-1-5-32-545

windbind_krb:
  realms:
    kdc:
      - kerberos.mit.edu:88
      - kerberos-1.mit.edu:88
      - kerberos-2.mit.edu:88
    admin_server: kerberos.mit.edu
    default_domain: mit.edu
  domain_realms:
    - .mit.edu
    - mit.edu
```

# Example Playbook

```yml
- hosts: winbind
  become: yes
  roles:
    - City-of-Bloomington.winbind
```

# Copying and License

This material is copyright 2016 City of Bloomington, Indiana
It is open and licensed under the GNU General Public License (GLP) v3.0 whose full text may be found at:
https://www.gnu.org/licenses/gpl.txt
