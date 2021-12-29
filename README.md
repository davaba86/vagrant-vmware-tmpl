# vagrant-vmware-tmpl

## TL;DR

This Vagrant is meant to be used as a general template, allowing you to generate one or more VM's with ease.

Also the ssh-config is copied into your ~/.ssh/config.d/, allowing you to ssh without the vagrant command. This however does require that you create the config.d path manually before.

## Requirements

```bash
mkdir ~/.ssh/config.d
```

Vagrant Plugins:

- vagrant-hostmanager
- vagrant-vmware-desktop

## How to Run

```bash
vagrant up
```

```bash
vagrant destroy --force
```
