# LDAP, LDAPS, and the Global Catalog

I understand **LDAP** as the protocol used to look up and work with directory objects, such as users and groups. **LDAPS** is LDAP over a TLS-protected connection, which protects directory queries and credentials in transit. LDAP on TCP 389 does not automatically mean encrypted traffic; StartTLS can also protect an LDAP connection when configured. LDAPS normally uses TCP 636 and needs an appropriate certificate on the domain controller.

For example, an LDAP filter can select user objects in HR:

```powershell
Get-ADObject -LDAPFilter '(&(objectClass=user)(department=HR))' -Properties department |
  Select-Object Name,department
```

In LDAP filter syntax, `&` means AND, `|` means OR, and `!` means NOT. These are ways to combine conditions in a directory query.

A **Global Catalog** gives a forest-wide view containing selected attributes of objects from its domains. In this one-domain lab, `Get-ADForest` identified `DC.corp.local` as a global catalog. Standard GC LDAP uses TCP 3268; GC over TLS uses 3269. The supplied screenshots do not show a port test, `ldp.exe` connection, or a failed LDAPS connection, so those are not reported as results.

Source for ports and TLS: [Microsoft Learn — AD DS ports](https://learn.microsoft.com/en-us/openspecs/windows_protocols/MS-ADTS/1010e59a-cf64-410c-a05c-a3a6c715261a) and [LDAP certificate configuration](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/configure-ldap-signing-certificates).
