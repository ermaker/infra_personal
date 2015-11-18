# Infra Personal

My personal infra

# Prerequisites

* [Vagrant]: See [here][Vagrant Install] for installation.
* A key pair for ssh access: `keys/public`, `keys/private`.

# Usage

Build up:

```sh
$ vagrant up
```

SSH access:

```sh
$ ssh -i [private key file] vagrant@[host ip] -p 3333
```

[Vagrant]: http://vagrantup.com
[Vagrant Install]: https://docs.vagrantup.com/v2/installation/index.html