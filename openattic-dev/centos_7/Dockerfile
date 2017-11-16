# OpenATTIC Dev docker image

FROM centos/systemd
MAINTAINER Ricardo Marques "rimarques@suse.com"

ADD entrypoint.sh pgsql_setup.sh service_alias.sh /

# Activate EPEL Repository
RUN yum install -y epel-release

#RUN zypper ar http://download.opensuse.org/repositories/filesystems:/ceph:/luminous/openSUSE_Leap_42.3/filesystems:ceph:luminous.repo
#RUN zypper ar http://download.opensuse.org/repositories/filesystems:/openATTIC:/3.x/openSUSE_Leap_42.3/filesystems:openATTIC:3.x.repo
#RUN zypper --gpg-auto-import-keys ref && \
#RUN yum install -y krb5 && \
RUN yum install -y mercurial git-core nodejs npm bridge-utils bzip2 dbus systemd \\
python-django-1.6.11.6 \\
django-filter djangorestframework djangorestframework-bulk m2crypto memcached \\
mod_wsgi ntp numpy policycoreutils-python pygobject2 python-dbus python-configobj \\
python-django python-imaging python-memcached python-netaddr \\
python-netifaces python-pam python-psycopg2 python-pyudev python-requests \\
python-simplejson policycoreutils python-rtslib \\
udisks2 wget net-tools httpd policycoreutils-python  \\
cronie bc python-coverage \\
nfs-utils postgresql-server \\
postgresql shadow-utils samba python-mock python-inotify && \\
#selinux-tools cron vlan smtp_daemon
yum install -y ceph-common python-rados && \\
yum install -y python-pip && \\
pip install django-filter==0.7 && \\
pip install djangorestframework==2.4.4 && \\
pip install djangorestframework-bulk && \\
pip install requests-aws && \\
yum clean all

#RUN (cd /usr/lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
#systemd-tmpfiles-setup.service ] || rm -f $i; done); \
#rm -f /usr/lib/systemd/system/multi-user.target.wants/*;\
#rm -f /usr/etc/systemd/system/*.wants/*;\
#rm -f /usr/lib/systemd/system/local-fs.target.wants/*; \
#rm -f /usr/lib/systemd/system/sockets.target.wants/*udev*; \
#rm -f /usr/lib/systemd/system/sockets.target.wants/*initctl*; \
#rm -f /usr/lib/systemd/system/basic.target.wants/*;\
#rm -f /usr/lib/systemd/system/anaconda.target.wants/*;

#CMD ["/usr/sbin/init"]

RUN /pgsql_setup.sh init_db

RUN groupadd -r openattic; usermod -a --groups openattic apache; \\
useradd -r -g openattic -d /var/lib/openattic -s /bin/bash -c "openATTIC System User" openattic; \\
usermod -a --groups apache openattic

EXPOSE 5432 80

VOLUME ["/srv/openattic", "/sys/fs/cgroup", "/etc/ceph"]

ENTRYPOINT ["/entrypoint.sh"]
