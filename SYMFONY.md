# Symfony



## Envoyer des emails

1. Dans le fichier app/config/parameters.yml
    ```bash
parameters:
    ...
    mailer_transport: mail
    mailer_host: 127.0.0.1
    mailer_port: 1025
    mailer_user: null
    mailer_password: null
    ...

    ```
2. Dans le fichier app/config/config_dev.yml
    ```bash
swiftmailer:
    delivery_addresses: ['me@symfony.dev']
    ```


