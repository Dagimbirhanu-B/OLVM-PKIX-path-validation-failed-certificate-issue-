# OLVM-PKIX-path-validation-failed-certificate-issue-
![Screenshot](screenshot1.png)
## Why This Happens:

- The most common reason for this error is an expired or invalid certificate on your Oracle Linux Virtualization Manager server.

- It can also occur if the server's system time is incorrect and out of sync with the certificate's "valid from" and "valid to" dates.

- Sometimes, if a custom or replacement CA certificate was used and not installed correctly, you may see this message as well.
Cause                             |  Solution                                     
----------------------------------+-----------------------------------------------
Server time incorrect             |  Sync server time with NTP or set manually    
Certificate expired               |  Renew/regenerate and install new certificates
New or custom CA installed        |  Ensure correct certificate chain is trusted  
Browser/client not trusting cert  |  Import/download Engine CA certificate        
