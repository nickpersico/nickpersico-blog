# NickPersico.com Blog

My personal blog, run on DigitalOcean and powered by Ghost.

### Updating DigitalOcean with the latest code

It's hacky, but it seems to be the easiest way to run ghost locally and maintain
the integrity of DigitalOcean's one-click Ghost droplet.

Once you've made changes to the `huckleberry` theme and pushed the changes to Github,
you need to copy the changed theme files to the live Ghost directory:

1. SSH into the DigitalOcean droplet: `ssh root@DROPLET_IP_ADDRESS`
1. `cd ..`
1. `cd nickpersico-blog` to go into the Git repository for the blog.
1. Pull down the latest code you just committed.
1. Go back a directory (`cd ..`)
1. Sync the new theme file changes in the repository with the active Ghost directory: `rsync -r nickpersico-blog/content/themes/huckleberry/* var/www/ghost/content/themes/huckleberry/`

After reloading the blog, you should see the updated changes live in production.

### Restarting Ghost on DigitalOcean

1. SSH into the DigitalOcean droplet: `ssh root@DROPLET_IP_ADDRESS`
1. `cd ..`
1. `cd var/www/ghost`
1. `service ghost restart`


### Renewal cronjob syntax

The DigitalOcean droplet is running a cronjob every 30 days at 1:05AM to renew the certificate. `--force-renew` ignores the renewal date and forces the renewal to happen.

```
# Renew  SSL certificate via letsencrypt every 30 days at 1:05AM
5 1 30 * * sudo ~/.local/share/letsencrypt/bin/letsencrypt renew --force-renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```