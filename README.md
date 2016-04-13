# letsencrypt-systemd-nginx
This is a small systemd unit for automating the Let's Encrypt certificate renewal for the nginx web server.
It runs monthly and just executes

    letsencrypt renew --standalone --keep-until-expiring

As you can see, it uses the standalone authenticator, because letsencrypt nginx support is incomplete.
This requires the web server to be stopped for a couple of seconds during renewal (once every two months).
