---
layout: post
title:  "How to renew CA certificate of PiVPN (OpenVPN)"
date:   2019-07-22 19:19:00 +0200
categories: openvpn pivpn
---


TL;DR If suddenly you cannot connect to your OpenVPN server based on PiVPN (or other), it is probably because of the CA certificate has expired.

Do:
```
cat /var/log/openvpn.log
```


If you find an output similar to the following, it means that (probably) the certificate has expired
```
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 VERIFY ERROR: depth=0, error=CRL has expired: CN=profile
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 OpenSSL: error:14089086:SSL routines:ssl3_get_client_certificate:certificate verify failed
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 TLS_ERROR: BIO read tls_read_plaintext error
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 TLS Error: TLS object -> incoming plaintext read error
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 TLS Error: TLS handshake failed
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 Fatal TLS error (check_tls_errors_co), restarting
Jul 22 18:52:44 raspberrypiserver ovpn-server[434]: 238.143.30.107:47626 SIGUSR1[soft,tls-error] received, client-instance restarting

```

Verify the certificate expiration date by typing
```
cd /etc/openvpn
sudo openssl crl -in crl.pem -text
```

which will output
```
Certificate Revocation List (CRL):
        Version 2 (0x1)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: /CN=ChangeMe
        Last Update: Jan 21 18:03:50 2019 GMT
        Next Update: Jul 20 18:03:50 2019 GMT
        CRL extensions:...
        ...
```

If the field `Next Update` indicates a date earlier than today, then the CA certificate has expired.

To renew it just do:

```
cd /etc/openvpn/easy-rsa
sudo ./easyrsa gen-crl
sudo cp pki/crl.pem /etc/openvpn/crl.pem
sudo systemctl restart openvpn
```

Done.
Enjoy!