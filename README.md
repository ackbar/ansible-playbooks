# ansible-playbooks
These ansible playbooks are meant to be used for deployment of LXC containers on a remote host. The [ansible-lxc-remote](https://github.com/Gu1/ansible-lxc-remote) plugin is needed to pass commands to the containers, which allows to not configure any SSH daemon inside those.

The host can run any Linux distribution as long as the kernel is recent enough for some LXC/namespaces features (3.18+) and as long as the lxc userland tools are at least at version 1.0. However, the current version of the playbooks assume the host to be running a Debian-like distribution and depends on apt for packages installation. The containers will use Ubuntu 14.04 LTS.

First, use the **lxc_create.yml** playbook to create a new container on the host system. Three parameters are mandatory:

  * name: the name of the LXC,
  * size: the size on the filesystem you would allocate to the container,
  * ipv4: an IPv4 address to be used by the container.

This playbook installs first some required packages on the host, then creates the LXC using LVM as a storage backend. The configuration of the container itself contains some hardening.

There are two roles defined at the moment:

  * **common**: defines common parameters among all the Linux systems which are deployed using ansible,
  * **lxc**: hardens the base system a little bit by disabling useless services, removing packages and the default "ubuntu" user.

More roles will come to specialize containers.
LXC (générique)
-> lxc-create.yml -e "name= size= ipv4="
-> lxc-minimize.yml
-> lxc-unused-services.yml
-> ubuntu-user.yml

# Clean /dev
chroot $rootfs /bin/bash -c 'cd /dev; rm agpgart audio* core dsp* loop* *mem midi* mixer* mpu401* port ram* rmidi* sequencer smpte* sndstat tty{0,2,3,4,5,6,7,8,9}'

# Clean upstart jobs
chroot $rootfs /bin/bash -c 'cd /etc/init; for job in console*.conf control-alt-delete.conf dmesg.conf failsafe.conf flush-early-job-log.conf hostname.conf hwclock*.conf module-init-tools.conf network*.conf plymouth*.conf procps.conf mountall*.conf mounted-{debugfs,dev,run}.conf resolvconf.conf rsyslog.conf tty{2,3,4,5,6}.conf udev*.conf upstart*.conf ureadahead*.conf; do mv $job $job.disabled; d

firewall
sysctl ?

Host
-> host-minimize.yml
-> host-unused-services.yml
-> networking.yml
-> ntp.yml

firewall
sysctl

Common
-> apt-recommends.yml
-> mirror.yml
-> set_hosts.yml
-> set_hostname.yml

Locales ?
chroot $rootfs /usr/sbin/locale-gen en_US.UTF-8 >/dev/null 2>&1
chroot $rootfs /usr/sbin/update-locale LANG=en_US.UTF-8


Spécialisation
-> APT-CACHER-NG
-> SYSLOG
-> DNS
-> SMTP
-> HTTP
-> HTTP+PHP
-> HTTP+Python
-> PROXY
-> DB PG
-> IMAP
-> GIT
-> IRC
-> BACKUP
