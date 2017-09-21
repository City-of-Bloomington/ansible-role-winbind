# City-of-Bloomington.winbind

Install winbind, and join a linux host to an Active Directory domain

## Dependencies

City-of-Bloomington.linux

# Requirements

This role does not cover the Windows Active Directory setup required.
You must have already created the groups used for this server in Active Directory.

# Group Mapping

This role allows for unlimited number of group mappings.  Group maps have
been split into Admin groups and User groups.  This lets you declare a
common set of Admin groups in group_vars, while setting per-host User groups
in host_vars.

This role expects to use Nested Groups on the local machine.  For us, this
makes it easier to manage complicated group permissions from Active Directory.
If you want to map AD groups directly to unix groups on the host machine, you
will need to modify the groups.yml task file. The rest of the Samba, kerberos,
and Winbind configuration should be the same.


# Role Variables

Available variables with example values

```yml
winbind_domain: ATHENA.MIT.EDU
winbind_workgroup: ATHENA

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

# This must be an Active Directory user with permission to join machines
# to the domain
winbind_domain_admin:
  user: "Adminstrator"
  pass: "{{ vault_winbind_domain_admin_pass }}"

# If you need to lookup a group SID the easiest way to do so is via the command
# line: wbinfo -n <group name>
#
# This script uses "Nested Groups" to map AD group users to unix groups.
#
# I *highly* recommend choosing local names for the ntgroups that do not
# exist in your Active Directory.  This will avoid future confusion with
# group membership
winbind_groupmap_admins:
  - { ntgroup: "Admins", unixgroup: "sudo",  domain_sid: "S-1-5-21-1004336348-1177238915-682003330-512" }

winbind_groupmap_users:
  - { ntgroup: "Staff",  unixgroup: "staff", domain_sid: "S-1-5-32-545" }

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
It is open and licensed under the GNU General Public License (GPL) v3.0 whose full text may be found at:
https://www.gnu.org/licenses/gpl.txt
