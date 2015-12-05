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
* each env file has an example file: `.env.*` -> `.env.*.example`

## Build Up

```sh
$ vagrant up
```

## SSH Access

```sh
$ ssh -i [PRIVATE_KEY_FILE] vagrant@[HOST_IP] -p [YOUR_SSH_PORT]
```

[Vagrant]: http://vagrantup.com
[Vagrant Install]: https://docs.vagrantup.com/v2/installation/index.html
[vagrant-env]: https://github.com/gosuri/vagrant-env