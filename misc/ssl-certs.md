# SSL/TLS Certificates

X.509 is an ITU standard defining the format of public key certificates. X.509 are used in TLS/SSL, which is the basis for HTTPS. An X.509 certificate binds an identity to a public key using a digital signature. A certificate contains an identity (hostname, organization, etc.) and a public key (RSA, DSA, ECDSA, ed25519, etc.), and is either signed by a Certificate Authority or is Self-Signed.

## Self-Signed Certificates
### Way 1
#### Generate CA
1. Generate RSA Key
```bash
openssl genrsa -aes256 -out ca-key.pem 4096
```
2. Generate a public CA Cert
```bash
openssl req -new -x509 -sha256 -days 3650 -key ca-key.pem -out ca.pem
```

#### Optional Stage: View Certificate's Content
```bash
openssl x509 -in ca.pem -text
openssl x509 -in ca.pem -purpose -noout -text
```

#### Generate Certificate
1. Create a RSA key
```bash
openssl genrsa -out nginx-selfsigned-key.pem 4096
```
2. Create a Certificate Signing Request (CSR)
```bash
openssl req -new -sha256 -subj "/CN=localhost" -key nginx-selfsigned-key.pem -out nginx-selfsigned.csr
```
3. Create a `extfile` with all the alternative names. Manually Create it to prevent errors when reading extfile.ext. Manually type it so encoding does not change(UTF 8 is the correct encoding).

create a new file named extfile.ext and put the code inside
```
subjectAltName = DNS:localhost
extendedKeyUsage = serverAuth
keyUsage = digitalSignature
```


4. Create the normal certificate
```bash
openssl x509 -req -sha256 -days 3650 -in nginx-selfsigned.csr -CA ca.pem -CAkey ca-key.pem -out nginx-selfsigned-crt.pem -extfile extfile.ext -CAcreateserial
```

5. Upload a full chain certificate
```bash
cat nginx-selfsigned-crt.pem > nginx-selfsigned-fullchain-crt.pem
```

```bash
cat ca.pem >> .\nginx-selfsigned-fullchain-crt.pem
```

6. Generate a strong Diffie-Hellman group
```bash
openssl dhparam -out dhparam.pem 4096
```
make sure the full chain and certificate is in the utf8 format
### Way 2
#### Ext file setup
1. create a new file named extfile.ext and put the code inside
```
distinguished_name = req_distinguished_name
req_extensions = v3_req

[req_distinguished_name]
countryName = Country Name (2 letter code)
countryName_default = US
stateOrProvinceName = State or Province Name (full name)
stateOrProvinceName_default = YourState
localityName = Locality Name (e.g., city)
localityName_default = YourCity
organizationName = Organization Name (e.g., company)
organizationName_default = YourOrganization
commonName = Common Name (e.g., your domain or localhost)
commonName_max = 64

[v3_req]
subjectAltName = @alt_names
extendedKeyUsage = serverAuth
keyUsage = digitalSignature

[alt_names]
DNS.1 = localhost
```

#### Generate ssl certificate and Key 
1. Generate ssl certificate and key
```bash
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt -config extfile.ext
```
2. Generate a strong Diffie-Hellman group
```bash
openssl dhparam -out dhparam.pem 4096
```

https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-debian-10#creating-a-configuration-snippet-pointing-to-the-ssl-key-and-certificate

## Certificate Formats

X.509 Certificates exist in Base64 Formats **PEM (.pem, .crt, .ca-bundle)**, **PKCS#7 (.p7b, p7s)** and Binary Formats **DER (.der, .cer)**, **PKCS#12 (.pfx, p12)**.

### Convert Certs

COMMAND | CONVERSION
---|---
`openssl x509 -outform der -in cert.pem -out cert.der` | PEM to DER
`openssl x509 -inform der -in cert.der -out cert.pem` | DER to PEM
`openssl pkcs12 -in cert.pfx -out cert.pem -nodes` | PFX to PEM

## Verify Certificates
`openssl verify -CAfile ca.pem -verbose cert.pem`

## Install the CA Cert as a trusted root CA

### On Debian & Derivatives
- Move the CA certificate (`ca.pem`) into `/usr/local/share/ca-certificates/ca.crt`.
- Update the Cert Store with:
```bash
sudo update-ca-certificates
```

Refer the documentation [here](https://wiki.debian.org/Self-Signed_Certificate) and [here.](https://manpages.debian.org/buster/ca-certificates/update-ca-certificates.8.en.html)

### On Fedora
- Move the CA certificate (`ca.pem`) to `/etc/pki/ca-trust/source/anchors/ca.pem` or `/usr/share/pki/ca-trust-source/anchors/ca.pem`
- Now run (with sudo if necessary):
```bash
update-ca-trust
```

Refer the documentation [here.](https://docs.fedoraproject.org/en-US/quick-docs/using-shared-system-certificates/)
### On Arch
System-wide – Arch(p11-kit)
(From arch wiki)
- Run (As root)
```bash
trust anchor --store myCA.crt
```
- The certificate will be written to /etc/ca-certificates/trust-source/myCA.p11-kit and the "legacy" directories automatically updated.
- If you get "no configured writable location" or a similar error, import the CA manually:
- Copy the certificate to the /etc/ca-certificates/trust-source/anchors directory.
- and then
```bash 
update-ca-trust
```
wiki page  [here](https://wiki.archlinux.org/title/User:Grawity/Adding_a_trusted_CA_certificate)

### On Windows

Assuming the path to your generated CA certificate as `C:\ca.pem`, run:
```powershell
Import-Certificate -FilePath "C:\ca.pem" -CertStoreLocation Cert:\LocalMachine\Root
```
- Set `-CertStoreLocation` to `Cert:\CurrentUser\Root` in case you want to trust certificates only for the logged in user.

OR

In Command Prompt, run:
```sh
certutil.exe -addstore root C:\ca.pem
```

- `certutil.exe` is a built-in tool (classic `System32` one) and adds a system-wide trust anchor.

### On Android

The exact steps vary device-to-device, but here is a generalised guide:
1. Open Phone Settings
2. Locate `Encryption and Credentials` section. It is generally found under `Settings > Security > Encryption and Credentials`
3. Choose `Install a certificate`
4. Choose `CA Certificate`
5. Locate the certificate file `ca.pem` on your SD Card/Internal Storage using the file manager.
6. Select to load it.
7. Done!

## on IOS devices
The exact steps vary device-to-device, but here is a generalised guide:
1. Send ca.pem to your device.  Send email to yourself adding ca.pem as an attachment
2. Make sure to login to email via browser: safari/chrome.
3. Open the attachment(ca.pem), press install
4. Done!
