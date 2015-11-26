# Infra Personal

My personal infra

# Prerequisites

* [Vagrant]: See [here][Vagrant Install] for installation.
* [vagrant-env]:

```sh
$ vagrant plugin install vagrant-env
```

# Usage

## Configuration

* A key pair for ssh access: `keys/public`, `keys/private`.
* `.env`

```
LOG_PORT_80=[YOUR_PORT]
LOG_PORT_24284=[YOUR_PORT]
LOG_PORT_22=[YOUR_PORT]
LOG_CPUS=[YOUR_CPU_NUMBER]
LOG_MEMORY=[YOUR_MEMORY]
SERVICE_PORT_80=[YOUR_PORT]
SERVICE_PORT_22=[YOUR_PORT]
```

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
[vagrant-env]: https://github.com/gosuri/vagrant-env