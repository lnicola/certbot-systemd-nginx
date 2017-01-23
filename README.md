# certbot-systemd-nginx
This is a small `systemd` unit for automating the Certbot certificate renewal for the nginx web server.
It runs daily with a random delay and simply executes

    certbot renew

It uses the Certbot settings from the last execution. The `webroot` or `nginx` authenticators are
recommende since this unit no longer stops `nginx`. If you are using the `standalone` mode, `certbot`
will not be able to bind to the `HTTP` port.

Please see the Certbot documentation for information about migrating to another authenticator.

# Installation
## Arch Linux
On Arch Linux you can use the `certbot-systemd-nginx` AUR package.

## Other Linux with systemd
To use this on other Linux distributions that use `systemd`, you can copy the units to their usual
location:

    # cp certbot-nginx.{service,timer} /etc/systemd/system/
    # systemctl daemon-reload
    # systemctl start certbot-nginx.service # to run manually
    # systemctl enable --now certbot-nginx.timer # to use the timer
