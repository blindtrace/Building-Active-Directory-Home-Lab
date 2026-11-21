# Kerberos: from sign-in to service access

Kerberos is the authentication protocol used here to let a user reach a service without sending the account password to that service. The domain controller hosts the **Key Distribution Center (KDC)**.

1. **Initial authentication:** The client derives a key from the user's password and commonly proves knowledge of it with encrypted pre-authentication data. It does **not** send the plaintext password to the KDC. If the request succeeds, the KDC issues a **ticket-granting ticket (TGT)** plus a session key. The TGT lets the client request service tickets later.
2. **Request a service ticket:** The client presents the TGT and a fresh authenticator to the KDC and identifies the service it wants. The KDC issues a service ticket, sometimes called a **TGS ticket**. The client then presents that ticket to the target service. It does not need to send the user's password or long-term key with the request.
3. **Identify the service:** A **service principal name (SPN)** identifies the target service instance, for example `cifs/DC.corp.local`. The SPN helps the KDC locate the account running the service; the service ticket is protected with a key associated with that account. The SPN itself is not an encryption key.
4. **Protect the TGT:** **KRBTGT** is the domain account associated with the ticket-granting service. Its secret protects TGTs. It is not a general password for all services.

## The lab experiment

In the `ahmed.ali` session on `PC01`, `klist purge` cleared the ticket cache and `klist` reported zero cached tickets. After `dir \\DC\IT`, `klist` showed a `krbtgt/CORP.LOCAL` ticket and `cifs/DC` service ticket. On the domain controller, Event Viewer displayed Security events `4768` and `4769`. The screenshot shows the event IDs, not the detailed event fields. No IP-based NTLM fallback test or KRBTGT reset is claimed here.

![Ticket cache emptied](../images/47-kerberos%20purge%20tickets.png)
![TGT and CIFS ticket after accessing the share](../images/47-%20Kerberos%20after%20dir%20it%20and%20find%20tickets.png)
![Kerberos event list](../images/47-kerberos%20event%20code.png)

Sources: [Microsoft Learn — authentication service exchange](https://learn.microsoft.com/en-us/windows/win32/secauthn/authentication-service-exchange), [Kerberos authentication overview](https://learn.microsoft.com/en-us/windows-server/security/kerberos/kerberos-authentication-overview), and [KRBTGT account](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-default-user-accounts).
