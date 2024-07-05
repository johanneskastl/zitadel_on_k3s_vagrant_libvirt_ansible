# zitadel_on_k3s_vagrant_libvirt_ansible

Vagrant-libvirt setup that creates a VM with k3s and installs
[Zitadel](https://zitadel.com) into the cluster. It also installs a ApacheDS
LDAP server and creates some users.

Default OS is openSUSE Leap 15.6, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `opensuse/Leap-15.6.x86_64`, using
   `vagrant box add opensuse/Leap-15.6.x86_64`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Open the URL that Ansible printed out at the end of the provisioning. Use the
   credentials that were also printed. You need to change the admin password
   directly after the login, setting up 2FA is optional.
1. After logging in, click on **Default settings** on the upper right corner.
   Switch to **Identity Providers** and click on **Active Directory/LDAP**.
   Enter the following details:

   * Name `apacheds`
   * Servers `ldap://apacheds.apacheds:389`
   * BaseDN `DC=apacheds,DC=ojkastl,DC=de`
   * BindDN `uid=admin,ou=system`
   * Bind Password `dobby`
   * Userbase `OU=Users,DC=apacheds,DC=ojkastl,DC=de`
   * User filters `cn`
   * User Object Classes `user`
   * ID attribute `cn`

1. Log out and go back to the login screen. Click on the `apacheds` button and
   enter the credentials for one of the three users (`hpotter`, `hgranger` or
   `rweasley`, each with password `dobby`. You should be able to successfully
   log in and create a Zitadel account by providing the necessary information.
1. Party!

## Cleaning up

The VMs can be torn down after playing around using `vagrant destroy`. This will
also remove the kubeconfig file `ansible/k3s-kubeconfig`.
