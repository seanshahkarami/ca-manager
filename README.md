# CA Manager

This is meant to be a simple set of scripts for managing a CA along with application
keys and certificates. *It is primarily for learning purposes!* It is heavily inspired
by [easy-rsa](https://github.com/OpenVPN/easy-rsa) - which may be a better option
for serious work.

## Usage

### Building a CA

First, we need to generate the CA's key and certificate. To do this, run:

```sh
scripts/build-ca
```

You will be prompted for a password used to protect the CA's key. *Keep this
password safe as it will be used to sign other application certificates.*

You should now have the CA's key `keys/ca.key` and certificate `certs/ca.crt`.

### Generating an application key and certificate

First, we'll generate an application key and certificate signing request.

```sh
scripts/gen-req myapp
```

This will create a key `keys/myapp.key` and a certificate signing request
`reqs/myapp.csr`.

Now, we'll have the CA sign this request.

```sh
scripts/sign-req myapp
```

You will be prompted for the CA's key password to complete the signing process.
Once you have done this, the application's certificate will be output to
`certs/myapp.crt`.

### Printing certificate information

Sometimes it's useful to view *real* contents of a certificate. If you were to
cat a certificate directly, the output looks something like this:

```sh
$ cat certs/rabbitmq.crt
```

```
-----BEGIN CERTIFICATE-----
MIIDMDCCAhgCCQCgKekgnq/3LDANBgkqhkiG9w0BAQsFADBZMQswCQYDVQQGEwJV
UzERMA8GA1UECBMISWxsaW5vaXMxEDAOBgNVBAcTB0NoaWNhZ28xDzANBgNVBAMT
BlRlc3RDQTEUMBIGA1UECxMLRGV2ZWxvcG1lbnQwHhcNMTcwNjE1MTIyNjA4WhcN
MTgwNjE1MTIyNjA4WjBbMQswCQYDVQQGEwJVUzERMA8GA1UECBMISWxsaW5vaXMx
EDAOBgNVBAcTB0NoaWNhZ28xETAPBgNVBAMTCHJhYmJpdG1xMRQwEgYDVQQLEwtE
ZXZlbG9wbWVudDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOlBeOyH
CQWuVSoNnMojjeR/aC/4rbgdUBqniQP6a8z5E+2GDrjl4YX5NrWPDK3lUuUIncAC
mDJ1YkJhV00636faqyy1U0/L2d83ZCJ5e9ClLhhs8vI6aN8GppMp0gmyti64307L
//+YnCcgo5OJzjeGvSLQcj1efObf25kw4Vmz0tNPyDmpLA/5yby+1xUx5Rj0PTX0
Wh1uNMKwA2P+t8OCzOuHScWLDMRlaBoVN6gvrwdDyb/kjEapQHqRXl/u1cNA6xFL
uNEVj4sXK/5MA/xdO+DdhXfH78PbnHN8w7F+FDaHuxOUcUjR8EzqDv/uQC8+KBSo
bXJqWbJmAwBvuIkCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAckqhKM5b3n1x7+PT
nrkWIoFM8t1ViIG899JSal8hweSn51v9hVUSJjNK/hgqIplNvWSw9UVs6tTxV9X1
r6YkDvVDVuTZFKTHes1SG332P6zNFSsswOtQtuZAXhmVnuCrU6V6W2ef2MHopiCW
H46cyBKx8FGSrbcWoQTOAzC84q1JLhDGPFkic8sXjdhWQEQ4dyTv+PPxqMozBtBt
lG0NaymUv818Dori+DKG1Hh80TLH6NlQmfz5ipzznKx4JJjxzny5mOSdz2UOvD4w
p+YttdxRgSdtEC3ipA/J3Ni6tv1F+i3Px02VU8jPsBM+2e9AS1MKm2sqN8m3et+F
HKvKSg==
-----END CERTIFICATE-----
```

Not so useful. Instead, you can use `scripts/print-cert` to get a dump of the
unpacked contents.

```sh
$ scripts/print-cert rabbitmq
```

```
Certificate:
    Data:
        Version: 1 (0x0)
        Serial Number:
            a0:29:e9:20:9e:af:f7:2c
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=US, ST=Illinois, L=Chicago, CN=TestCA, OU=Development
        Validity
            Not Before: Jun 15 12:26:08 2017 GMT
            Not After : Jun 15 12:26:08 2018 GMT
        Subject: C=US, ST=Illinois, L=Chicago, CN=rabbitmq, OU=Development
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
            RSA Public Key: (2048 bit)
                Modulus (2048 bit):
                    00:e9:41:78:ec:87:09:05:ae:55:2a:0d:9c:ca:23:
                    8d:e4:7f:68:2f:f8:ad:b8:1d:50:1a:a7:89:03:fa:
                    6b:cc:f9:13:ed:86:0e:b8:e5:e1:85:f9:36:b5:8f:
                    0c:ad:e5:52:e5:08:9d:c0:02:98:32:75:62:42:61:
                    57:4d:3a:df:a7:da:ab:2c:b5:53:4f:cb:d9:df:37:
                    64:22:79:7b:d0:a5:2e:18:6c:f2:f2:3a:68:df:06:
                    a6:93:29:d2:09:b2:b6:2e:b8:df:4e:cb:ff:ff:98:
                    9c:27:20:a3:93:89:ce:37:86:bd:22:d0:72:3d:5e:
                    7c:e6:df:db:99:30:e1:59:b3:d2:d3:4f:c8:39:a9:
                    2c:0f:f9:c9:bc:be:d7:15:31:e5:18:f4:3d:35:f4:
                    5a:1d:6e:34:c2:b0:03:63:fe:b7:c3:82:cc:eb:87:
                    49:c5:8b:0c:c4:65:68:1a:15:37:a8:2f:af:07:43:
                    c9:bf:e4:8c:46:a9:40:7a:91:5e:5f:ee:d5:c3:40:
                    eb:11:4b:b8:d1:15:8f:8b:17:2b:fe:4c:03:fc:5d:
                    3b:e0:dd:85:77:c7:ef:c3:db:9c:73:7c:c3:b1:7e:
                    14:36:87:bb:13:94:71:48:d1:f0:4c:ea:0e:ff:ee:
                    40:2f:3e:28:14:a8:6d:72:6a:59:b2:66:03:00:6f:
                    b8:89
                Exponent: 65537 (0x10001)
    Signature Algorithm: sha256WithRSAEncryption
        72:4a:a1:28:ce:5b:de:7d:71:ef:e3:d3:9e:b9:16:22:81:4c:
        f2:dd:55:88:81:bc:f7:d2:52:6a:5f:21:c1:e4:a7:e7:5b:fd:
        85:55:12:26:33:4a:fe:18:2a:22:99:4d:bd:64:b0:f5:45:6c:
        ea:d4:f1:57:d5:f5:af:a6:24:0e:f5:43:56:e4:d9:14:a4:c7:
        7a:cd:52:1b:7d:f6:3f:ac:cd:15:2b:2c:c0:eb:50:b6:e6:40:
        5e:19:95:9e:e0:ab:53:a5:7a:5b:67:9f:d8:c1:e8:a6:20:96:
        1f:8e:9c:c8:12:b1:f0:51:92:ad:b7:16:a1:04:ce:03:30:bc:
        e2:ad:49:2e:10:c6:3c:59:22:73:cb:17:8d:d8:56:40:44:38:
        77:24:ef:f8:f3:f1:a8:ca:33:06:d0:6d:94:6d:0d:6b:29:94:
        bf:cd:7c:0e:8a:e2:f8:32:86:d4:78:7c:d1:32:c7:e8:d9:50:
        99:fc:f9:8a:9c:f3:9c:ac:78:24:98:f1:ce:7c:b9:98:e4:9d:
        cf:65:0e:bc:3e:30:a7:e6:2d:b5:dc:51:81:27:6d:10:2d:e2:
        a4:0f:c9:dc:d8:ba:b6:fd:45:fa:2d:cf:c7:4d:95:53:c8:cf:
        b0:13:3e:d9:ef:40:4b:53:0a:9b:6b:2a:37:c9:b7:7a:df:85:
        1c:ab:ca:4a
```

## Useful References

If you're interested in learning more about the underlying details involved, here are some resources you may find useful.

* [Public Key Cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography). I like the
opening paragraph of the Wikipedia page as it immediately mentions two primary
functions of public key cryptography: authentication / signing / verification and encryption / decryption. I think it's useful to have these two distinct applications explicitly mentioned and may clarify how these techniques can be used within TLS/SSL.

* [X.509](https://en.wikipedia.org/wiki/X.509). This is a reference for the particular standard we're using for certificates. If you're interested in understanding the contents and background of these, this is a good place to start.
