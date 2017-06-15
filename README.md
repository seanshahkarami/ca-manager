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
