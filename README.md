# cloud-gateway

Setup gateway host in cloud provider (only DigitalOcean supported for the moment) with `ipsec` (libreswan) and `openvpn` (pritunl).

For DigitalOcean droplet setup `DO_API_TOKEN` environment variable should contain API token.

IPsec configuration sample could be found [here](https://raw.githubusercontent.com/oukooveu/cloud-gateway/master/tests/host_vars/test-gw-1.yaml).

[Pritunl](https://pritunl.com/) should be configured manually (because of API documentation absence).

NAT masquerading for [routes](https://docs.pritunl.com/docs/routing) can be disable with:
```
use pritunl
db.servers.updateOne(
    {_id : ObjectId("<ID>")},
    {$set : {"routes.<INDEX>.nat" : "true"}}
)
```
