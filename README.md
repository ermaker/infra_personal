# Infra Personal

My personal infra

# Prerequisites

* [Vagrant]: See [here][Vagrant Install] for installation.

# Usage

## Configuration

* A key pair for ssh access: `keys/public`, `keys/private`.
* `log/.env.fluentd` and `service/.env.fluentd`

```
SHARED_KEY=[YOUR_SHARED_SECRET_KEY]
```

* `service/.env.mshard`

```
PUSHBULLET_ACCESS_TOKEN=[YOUR_PUSHBULLET_ACCESS_TOKEN]
SECRET_KEY_BASE=[YOUR_SECRET_KEY_BASE]
```

## Build Up

```sh
$ vagrant up
```

## SSH Access

```sh
$ ssh -i [private key file] vagrant@[host ip] -p 3333
```

[Vagrant]: http://vagrantup.com
[Vagrant Install]: https://docs.vagrantup.com/v2/installation/index.html