stacki-backend
=========

Ansible tooling for hosts kicked by [Stacki](http://www.stacki.com/), a dandy
little kickstart configurator and bootstrapping appliance for CentOS

Stacki is a feature-full configurator that presumes cluster-dependence after OS
bootstrapping is complete. While this is an elegant solution for many, you may
need the ping and prompt without these dependencies because you have other tools
for OS and app configuration and deployment (e.g., curated package
repositories). This role expresses the steps I took to decouple the target metal
from the cluster.


Requirements
------------


Role Variables
--------------

- set ntp server domain name in stacki-backend.yml
- set nameserver ip addresses in stacki-backend.yml
- set dns search hints in stacki-backend.yml
- set root and admins'password shadow/hashes and authorized ssh pubkeys in
  vars/main.yml



Example Playbook
----------------

from stacki-backend.yml
````yaml
---

- hosts: all

  roles:
    - stacki-backend

  vars:
    kickedbystacki: true
    env:
      ns:
        - 8.8.8.8
        - 4.2.2.2
      hints:
        - subdomain.example.com
        - example.com
      ntp: pool.ntp.org

...

````


Incantation
----------------


**ssh as root with a password and execute on the host groups named in the stacki-backend.yml playbook file**
````shell
ansible-playbook \
  --inventory hosts-stacki-backend.txt \
  --user root --ask-pass \
  stacki-backend.yml
````

**ssh as root without a password (because another auth method is enabled) and execute on the comma-separated list (note the trailing comma) any tasks or roles specified in the stacki-backend.yml playbook file**
````shell
ANSIBLE_CONFIG=ansible.cfg ansible-playbook \
  --inventory 'app001.vdc1.example.com,' \
  --user root \
  stacki-backend.yml
````

License
-------

BSD

Author Information
------------------

[Kenneth Bingham](http://w.qrk.us) <w@qrk.us> (2017)


