# Infra Personal

My personal infra

# Prerequisites

* [Vagrant]: See [here][Vagrant Install] for installation.
* [vagrant-env]:

```sh
$ vagrant plugin install vagrant-env
```

# Running Services

* log VM
  * es: for logging
  * kibana: frontend for logs
  * fluentd: for forwarding logs to es
  * nginx: for static web pages
  * nginx-proxy: proxy of all services in log VM
* service VM
  * iptime_rebooter: Reboots the iptime router every 12 hours
  * redis: for test and mshard
  * mongo: for test
  * mshard: for test
  * nginx: for test
  * nginx-proxy: for test and proxy
  * fluentd: for forwarding logs to log VM

# Usage

## Configuration

* A key pair for ssh access: `keys/public`, `keys/private`.
* Make .env.* files:
  * each env file has an example file: `.env.*` -> `.env.*.example`

## Build Up

```sh
$ vagrant up
```

## SSH Access

```sh
$ ssh -i [PRIVATE_KEY_FILE] vagrant@[HOST_IP] -p [YOUR_SSH_PORT]
```

PRIVATE_KEY_FILE: `keys/private`

[Vagrant]: http://vagrantup.com
[Vagrant Install]: https://docs.vagrantup.com/v2/installation/index.html
[vagrant-env]: https://github.com/gosuri/vagrant-env