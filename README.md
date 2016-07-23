# certbot-systemd-nginx
This is a small `systemd` unit for automating the Certbot certificate renewal for the nginx web server.
It runs monthly and simply executes

    certbot renew --standalone

It uses the standalone authenticator, because Certbot nginx support is incomplete. This requires the
web server to be stopped for a couple of seconds during each run (monthly, but can be overridden) and
a bit more during renewal (once every two months).

# Installation
## Arch Linux
On Arch Linux you can use the `certbot-systemd-nginx` AUR package.

## Other Linux with systemd
To use this on other Linux distributions that use `systemd`, you can copy the units to their usual
location:

    # cp certbot-nginx.{service,timer} /etc/systemd/system/
    # systemctl daemon-reload
    # systemctl start certbot-nginx.service # to start manually
    # systemctl enable --now certbot-nginx.timer # to use the timer
