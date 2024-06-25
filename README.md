# priobike-docker-registry

## Running the registry on `bikenow.vkw.tu-dresden.de`

Note that the htpasswd is managed by the NGINX on the VM. To see its configuration, run `sudo cat /etc/nginx/sites-enabled/default`.

Run the registry: 

```bash
sudo docker run -d -p 31337:5000 --restart=always --name registry -e REGISTRY_STORAGE_DELETE_ENABLED=true -e REGISTRY_STORAGE_CACHE_BLOBDESCRIPTOR=blah  -v /data/registry:/var/lib/registry registry:2
```

## Cleaning up the registry

It is recommended to add this command to a cronjob:

```bash
touch /home/admin/registry-gc.log
```

```bash
sudo docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml -m > /home/admin/registry-gc.log
```

`-m`: delete manifests that are not currently referenced via tags. 

The cronjob should run as root, e.g.:

```bash
sudo crontab -e
```

```bash
*/60 * * * * /usr/bin/docker exec registry bin/registry garbage-collect /etc/docker/registry/config.yml -m > /home/admin/registry-gc.log
```
## Setting up Certbot
https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal

## Setting up Nginx
https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04

you may need to disable ipv6 until ssl is set.
