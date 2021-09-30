# cloud-gateway


Setup gateway host in cloud provider (only DigitalOcean supported for the moment) with `ipsec` (libreswan) and `openvpn` (pritunl).

IPsec configuration sample could be found [here](https://raw.githubusercontent.com/oukooveu/cloud-gateway/master/tests/host_vars/test-gw-1.yaml).

[Pritunl](https://pritunl.com/) should be configured manually (because of API documentation absence).

Pritunl server adds NAT masquerading for [routes](https://docs.pritunl.com/docs/routing) by default, this can be disabled by direct MongoDB update:
```
use pritunl
db.servers.updateOne(
    { _id : ObjectId("<ID>") },
    { $set : { "routes.<INDEX>.nat" : false } }
)
```

## How to use
```
ansible-galaxy install -r requirements.yaml
pip install -r requirements.txt
ansible-playbook -i <inventory> site.yaml
```

## Tests

Only IPsec/BGP connectivity is testing.

### How to run tests locally

1. Install Vagrant: https://www.vagrantup.com/docs/installation;
1. Install VirtualBox: https://www.virtualbox.org/wiki/Downloads;
1. Run tests: `./tests/start`;
1. Play with created IPsec configuration;
1. Destroy test environment: `./tests/stop`.

### Versions were verified with tests

| Vagrant | Ansible (Core) | VirtualBox | CentOS   |
|---------|----------------|------------|----------|
| 2.2.15  | 4.2.0 (2.11.5) | 6.1.26     | 7.8.2003 |
| 2.2.3   | 2.7.5          | 5.2.22     | 7.6.1810 |
