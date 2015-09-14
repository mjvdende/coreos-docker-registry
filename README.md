# coreos-docker-registry

_Docker registry created for project [Scalable QA with Docker](https://github.com/xebia/scalable-qa-with-docker). Place contents from registry-contents in registry directory, see [releases](https://github.com/mjvdende/coreos-docker-registry/releases)_
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
    $ journalctl -u docker-registry -f

## Docker registry

- docker.service
- docker-registry.service - URL: [Registry](http://172.17.8.128:5000)
- file-server.service - URL: [file-server](http://172.17.8.128)

## Config

### Cloud-Config

To start our vm, we need to provide some config parameters in cloud-config format via the ```core01.user-data``` file.

More about using [cloud-config](https://coreos.com/os/docs/latest/cloud-config.html)
