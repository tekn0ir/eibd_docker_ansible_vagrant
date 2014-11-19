vagrant-ansible-docker-eibd
===================

A  ansible project for building an eibd docker image.

Usage
============

To run this demo, you need to install ansible and [vagrant](http://www.vagrantup.com) with vbguest plugin.

Then:

```
vagrant up
```

Go into vagrant shell:

```
vagrant ssh
```

Start the container:

```
sudo docker run -p 6720:6720 -it <image_name> /bin/bash
```

Start eibd in the container:

```
service eibd start
```

To detach from the docker container press CTRL-P + CTRL-Q

The vagrant should now have the eidb running within a docker container and the port 6720 is open