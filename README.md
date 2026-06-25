# Windows Server RDS Learning Path 13 — Join RDS01 to the AD Domain

**Level:** Beginner · **Module:** 13/70

## Goal
Join `RDS01` to `corp.lab` and verify trust before installing RDS roles.

## Setup
1. Resolve the domain and LDAP SRV records.
2. Compare time with DC01.
3. Join the domain using an interactive authorized credential.
4. Restart and sign in with a separate administrative identity.
5. Validate the secure channel, DC locator, computer object and Kerberos tickets.

```powershell
Resolve-DnsName corp.lab
Resolve-DnsName -Type SRV _ldap._tcp.dc._msdcs.corp.lab
w32tm.exe /stripchart /computer:dc01.corp.lab /samples:5 /dataonly
$Cred=Get-Credential
Add-Computer -DomainName corp.lab -Credential $Cred -Restart

# After restart
Test-ComputerSecureChannel -Verbose
nltest.exe /dsgetdc:corp.lab
klist.exe
Get-ADComputer RDS01 -Properties DNSHostName,OperatingSystem,PasswordLastSet
```

## Evidence
Save DNS/time baseline, join output, domain membership, secure-channel result, DC locator, tickets and the AD computer object under `evidence/`.

## Troubleshooting
Fix DNS, time, duplicate computer names and disabled objects before retrying. Do not install RDS roles while trust is unhealthy.

## Security
Use delegated join rights where available; never embed join credentials in scripts.

## Rollback
Unjoin only when intentionally rebuilding and then clean up the stale AD object.

## Next
`Windows-Server-RDS-Learning-Path-14-Move-RDS01-into-the-Servers-OU`
