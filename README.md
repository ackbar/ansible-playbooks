# ansible-playbooks

These ansible playbooks are meant to be used for deployment of LXC containers on a remote host. The [ansible-lxc-remote](https://github.com/ackbar/ansible-playbook) plugin is needed to pass commands to the containers, which allows to not configure any SSH daemon inside those.

The host can run any Linux distribution as long as the kernel is recent enough for some LXC/namespaces features (3.18+) and as long as the lxc userland tools are at least at version 1.0. However, the current version of the playbooks assume the host to be running a Debian-like distribution and depends on apt for packages installation. The containers will use Ubuntu 14.04 LTS.

First, use the **lxc_create.yml** playbook to create a new container on the host system. Three parameters are mandatory:

  * name: the name of the LXC,
  * size: the size on the filesystem you would allocate to the container,
  * ipv4: an IPv4 address to be used by the container.

This playbook installs first some required packages on the host, then creates the LXC using LVM as a storgae backend. The configuration of the container itself contains some hardening.

There are two roles defined at the moment:

  * **common**: defines common parameters among all the Linux systems which are deployed using ansible,
  * **lxc**: hardens the base system a little bit by disabling useless services, removing packages and the default "ubuntu" user.

More roles will come to specialize containers.
