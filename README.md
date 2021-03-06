# Vagrant project for OpenShift installation

Installs [OpenShift Origin](https://www.okd.io/) on a Vagrant managed VM with CentOS 7. The main motivation of using this instead of [minishift](https://github.com/minishift/minishift) is that the latter doesn't support [CRI-O](https://cri-o.io/) as of now.

## Features

- fully automatic but customisable installation

- defaults to CRI-O runtime

- default configuration for Vagrant on Libvirt/KVM/Linux

- configures nip.io as a default subdomain for OpenShift applications

## Usage

* Install Vagrant from https://www.vagrantup.com/downloads.html

* Get dependencies for Libvirt installation

        sudo yum install qemu libvirt libvirt-devel ruby-devel gcc qemu-kvm

* Get Vagrant Libvirt plugin

        vagrant plugin install vagrant-libvirt

* Get CentOS box for CentOS/7 and Libvirt provider (optional)

        vagrant box add centos/7 --provider libvirt

* Clone Git repo and run `vagrant up`

## Todo

- add VirtualBox specific configuration

- adapt local scripts to other platforms

- patches and PRs welcome

