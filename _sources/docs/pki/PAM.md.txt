# Pluggable Authentication Modules (PAM)

Pluggable Authentication Modules (PAM) is a suite of shared libraries, providing an abstraction layer between the various methods available that provide authentication and the applications that require authentication and would otherwise have to support those methods directly. With PAM, all that is necessary for these components to cooperate is that a specific PAM module be available for each of them. 

## SSH

The `pam_listfile.so` module authenticates users based on the contents of a specified file. For example, if username exists in a file `/etc/ssh/ssh.allow`, ssh will grant login access.

### Deny access
Append `/etc/pam.d/ssh`:

    auth required pam_listfile.so item=user sense=deny file=/etc/ssh/ssh.deny onerr=succeed

Add all usernames you wish to deny access for to a `/etc/ssh/ssh.deny` file.

### Allow access

Append `/etc/pam.d/ssh`:

    auth required pam_listfile.so item=user sense=allow file=/etc/ssh/ssh.allow onerr=fail

Add all usernames to allow access for to a `/etc/ssh/ssh.deny` file.

## SASL

[SASL](mailserver-mitigations:docs/SASL) can use different authentication methods. The default one is PAM (as configured in `/etc/conf.d/saslauthd`)

Create `/etc/pam.d/smtp`

    #%PAM-1.0
    auth            required        pam_unix.so
    account         required        pam_unix.so

