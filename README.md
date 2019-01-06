cloud-gateway
============

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

How to use
------------
```
ansible-galaxy install -r requirements.yaml
pip install -r requirements.txt
ansible-playbook -i <inventory> site.yaml
```
