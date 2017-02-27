Docker
=========
[![license][2i]][2p]

Install docker with docker-compose for a number of linux variants.

Usage
-----

All you need to do is add to your playbook and call from it:

``` yaml
- hosts: servers
    roles:
        - docker
```

Author Information
------------------

[Alejandro Baez][1]

[1]: https://keybase.io/baez
[2i]: https://img.shields.io/badge/license-BSD_2-blue.svg
[2p]: ./LICENSE
