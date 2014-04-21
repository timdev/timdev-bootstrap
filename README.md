# timdev's bootstrap role

## Introduction

This role is meant to be used a component in various configuration systems.  It's job is bring a Debian-type box
up to date, and perform some basic lock-down tasks.

This role is suitable for servers being administered by a small group of trusted admins.

It was developed against VPSes at linode, but should work just about anywhere.

It should be able to comfortably live as a git submodule under your project's ansible tree.

The role assumes you have a newly provisioned, completely bare, Debian 7.4 server.  The only user is root, and root has
a password, which you know.

The role does the following:

* Sets the hostname of the server to {{ inventory_hostname }}
* Creates a user named ansible, who is able to sude.
* Adds your local ~/.ssh/id_rsa.pub to ansible's authorized_keys (so you can `ssh ansible@your.host`
* Creates a set of users, all of which can sudo.  Does not set passwords, but adds keys (see below for configuration)
* Allows sudoing without a password.
* Performs a dist-upgrade
* Reboots the server, and waits for it to come back up.

## This role might as well be a playbook

Yes, it really could.  But it's packaged up as a role so it can be included in other repositories as a submodule.

The role provides it's own playbook: `bootstrap.play.yml`.

I typically add this repo as a submodule under roles (ie: roles/bootstrap), and then symlink the playbook to my root:

```
ln -s roles/bootstrap/bootstrap.play.yml bootstrap.yml
```



## Use case

I use this role in various provisioning systems.  It's meant to be nicely composable, since this is the stuff I do
pretty much every time I spin up a new VPS.

The scope of this playbook is deliberately limited to things you might do as root-with-a-password upon first logging
in to a new server.  Other tasks, like disabling root log-ins, or disabling password authentication are out of scope,
and don't work here, anyway (since once you do that, you'd need to change remote_user in the middle of a playbook, which
Ansible doesn't currently support).
)