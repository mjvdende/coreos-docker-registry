# coreos-docker-registry

_Current set up allocats 2 CPU and 2 GB of memory._

Given:

- [Vagrant](https://www.vagrantup.com/) + [VirtualBox](https://www.virtualbox.org/)

When:

    $ vagrant up

Then:

    start building!

It can take a while before services are started because docker is downloading images from the docker hub.
Therefor you can follow the progress of services booting when logging on to a core, for example core-01.

    $ vagrant ssh
    $ journalctl -u docker-registry-web.service -f

## core-01

- docker.service
- docker-registry.service - URL: [Registry](http://172.17.8.128:5000) (login:root-root)

## Config

### Cloud-Config

To start our vm, we need to provide some config parameters in cloud-config format via the ```core01.user-data``` file.

More about using [cloud-config](https://coreos.com/os/docs/latest/cloud-config.html)
