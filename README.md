# certbot-systemd-nginx

This is a small `systemd` unit for automating the Certbot certificate renewal for the nginx web server.
It runs daily with a random delay and simply executes

    certbot renew

It uses the Certbot settings from the last execution. The `webroot` or `nginx` authenticators are
recommende since this unit no longer stops `nginx`. If you are using the `standalone` mode, `certbot`
will not be able to bind to the `HTTP` port.

# Deprecation notice

This unit was originally written because the Certbot `nginx` authenticator was unreliable. The
only thing it does that is specific to that web server is telling it to reload its configuration.

In the meanwhile, `certbot-nginx` got fixed, and the Certbot authenticators know how to reload
the web server configuration. So there is nothing specific to `nginx` left for this unit to do.
Instead you can use [certbot-systemd][certbot-systemd] or the corresponding AUR package, to which
the author has no affiliation.

For more information, see [this PR][PR]. Apologies for the incovenience if you were using the AUR
package.

## Migration

If you were using the `webroot` authenticator and want to switch to `nginx`, run the following
command:

    # certbot renew --nginx --force-renewal

The new authenticator will be used for future renewals.

[certbot-systemd]: https://github.com/parchd-1/certbot-systemd/
[PR]: https://github.com/lnicola/certbot-systemd-nginx/pull/5

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

# Configuring nginx for webroot

Migrating to the `webroot` authenticator is pretty simple, but don't forget to make a backup of your
configuration file (`/etc/nginx/nginx.conf` or similar).

If you are hosting a site, edit the configuration file and add the following lines:

    location ~ /.well-known {
        allow all; # if using address restrictions
        auth_basic off; # using basic authentication
    }

If you're running a reverse proxy, pick a directory, create it and set it as root:

    location ~ /.well-known {
        root /var/www/html;
        allow all;
        auth_basic off;
    }

After making the changes, test and reload the new configuration:

    # nginx -t
    # systemctl reload nginx

and do a manual run of `certbot` to update its settings:

    # certbot certonly --webroot -w /var/www/html -d example.com --force-renewal

For more information, please see the Certbot or nginx documentation.
