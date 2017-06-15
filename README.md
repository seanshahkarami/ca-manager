# CA Manager

This is meant to be a simple set of scripts for managing a CA along with application
keys and certificates. *It is primarily for learning purposes!* It is heavily inspired
by [easy-rsa](https://github.com/OpenVPN/easy-rsa), which may be a better option
for serious work.

## Usage

### Building a CA

First, we need to generate the CA's key and certificate. To do this, run:

```sh
scripts/build-ca
```

You will be prompted for a password used to protect the CA's key file. *Keep
this password safe as it will be used to sign other application certificates.*
This will ensure the `certs`, `keys` and `reqs` directories are created.
