# Example: RabbitMQ with TLS/SSL

## Basic configuration (server authentication only)

The most basic configuration we can setup is to add server authentication. This
allow a client to verify that they are *actually* talking to the expected RabbitMQ
broker and provides encrypted communication between the client and server. It *does
not* use client side certificates for user login.

### 1. Generate and sign keys / cert for RabbitMQ broker

Assuming you've generated the CA key and certificate, you can run:

```sh
scripts/gen-req rabbitmq
scripts/sign-req rabbitmq
```

This will produce an application key `keys/rabbitmq.key` and certificate
`certs/rabbitmq.crt`.

In order to configure RabbitMQ to use TLS/SSL, we'll first copy over the
required files.

```sh
cp certs/ca.crt certs/rabbitmq.crt keys/rabbitmq.key /etc/rabbitmq/
```

Now, we'll create our config file `/etc/rabbitmq/rabbitmq.config`.

```
[
  {rabbit, [
     {ssl_listeners, [5671]},
     {ssl_options, [{cacertfile,"/etc/rabbitmq/ca.crt"},
                    {certfile,"/etc/rabbitmq/rabbitmq.crt"},
                    {keyfile,"/etc/rabbitmq/rabbitmq.key"}]}
   ]},
   {rabbitmq_management,
     [{listener, [{port, 15671},
                  {ssl, true},
                  {ssl_opts, [{cacertfile, "/etc/rabbitmq/ca.crt"},
                              {certfile, "/etc/rabbitmq/rabbitmq.crt"},
                              {keyfile, "/etc/rabbitmq/rabbitmq.key"}]}
                 ]}
     ]}
].
```

This configuration uses TLS/SSL for both messaging and the management
interface.
