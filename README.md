# OLVM-PKIX-path-validation-failed-certificate-issue-
![Screenshot](screenshot1.png)
![Screenshot](screenshot3.png)
## Why This Happens:

- The most common reason for this error is an expired or invalid certificate on your Oracle Linux Virtualization Manager server.

- It can also occur if the server's system time is incorrect and out of sync with the certificate's "valid from" and "valid to" dates.

- Sometimes, if a custom or replacement CA certificate was used and not installed correctly, you may see this message as well.

  ![Screenshot](screenshot2.png)

## how to solve
to see certificate details 
```
# Check Apache/engine certificate (main cert in most OLVM installs)
openssl x509 -in /etc/pki/ovirt-engine/certs/apache.cer -noout -dates

# Check CA certificate (can also be presented to clients)
openssl x509 -in /etc/pki/ovirt-engine/apache-ca.pem -noout -dates
# Check the Apache/Engine Server Certificate Validity:
openssl x509 -in /etc/pki/ovirt-engine/certs/apache.cer -noout -dates

```
here is the output i get from the server showing the error like in the screenshot
```

[ ~]$ # Check CA certificate (can also be presented to clients)
[~]$ openssl x509 -in /etc/pki/ovirt-engine/apache-ca.pem -noout -dates
notBefore=Sep 17 16:23:25 2024 GMT
notAfter=Sep 13 16:23:25 2044 GMT
[~]$ openssl x509 -in /etc/pki/ovirt-engine/apache-ca.pem -noout -dates
notBefore=Sep 17 16:23:25 2024 GMT
notAfter=Sep 13 16:23:25 2044 GMT

[~]$ openssl x509 -in /etc/pki/ovirt-engine/certs/apache.cer -noout -dates
notBefore=Sep 17 16:23:25 2024 GMT
notAfter=Oct 21 16:23:25 2025 GMT
[ ~]$
```
Apache/engine server certificate (/etc/pki/ovirt-engine/certs/apache.cer) is expired:

Valid from: Sep 17, 2024

Valid until: Oct 21, 2025

Today's date is October 23, 2025, so the certificate expired 2 days ago.
This is the direct cause of all the browser and PKIX errors you are seeing!
## hot to fix
# OLVM Certificate Renewal Guide

## Overview
This guide explains how to fix issues related to an **expired server certificate** in your **Oracle Linux Virtualization Manager (OLVM)**.  
The process involves **regenerating the server certificate** and **restarting services** to apply the changes.

---

## Issue Description
You might encounter login issues or security warnings when accessing the OLVM web interface.  
The error usually indicates that:

> The Certificate Authority (CA) is still valid, but the **server certificate** has expired.

This guide provides the steps to resolve this issue quickly.

---

## Step 1: Regenerate or Renew the Server Certificate

Run the following command on your OLVM Manager **as root**:
```
sudo engine-setup
```
During setup, you will be prompted about renewing expired keys and certificates. Choose 'Yes'.

This will reissue a fresh server certificate and update your configuration.

This ensures the new certificate is active and in use.
## Step 2: Restart OLVM Services After Regeneration
```
sudo systemctl restart ovirt-engine httpd
```
This ensures the new certificate is active.
---

## Step 3: Verify Access

After restarting the services:
1. Open your web browser.
2. Visit your OLVM Manager URL (e.g., `https://your-olvm-manager.example.com`).
3. Confirm that the previous SSL warnings are **gone** and login works normally.

---

## Summary
| Status | Description |
|--------|--------------|
| Issue | Expired server certificate while CA remains valid |
| Root Cause | Server certificate no longer trusted by browser |
| Fix | Regenerate certificate using `engine-setup` |
| Validation | Successful login without certificate warning |

---

## Notes
- This process **does not replace the CA** — it only refreshes the server’s SSL certificate.
- Always ensure system date and time are accurate before renewing certificates.
- If the issue persists, review `/var/log/ovirt-engine/setup/` for detailed logs.

---




