# openbook-heat

Description
===========
This is a collection of Heat templates for deploying Talligent's Openbook
billing/invoicing and customer lifecycle management software for OpenStack.
One template is for Openbook evaluation purposes (openbook-single.yaml),
while the other is for deploying a production-ready Openbook (openbook-cluster.yaml)

openbook-single
===============
This template deploys a single instance (m1.medium) into an OpenStack
environment.  It requires:
* an existing key pair (key_name)
* an OpenStack flavor that matches the m1.medium specs (flavor: defaults to 'm1.medium')
* an Ubuntu/CentOS server glance image
* a private neutron network (private_net: defaults to 'private')
* a public/external neutron network (public_net: defaults to 'public')
* **username and password to the Talligent Sharefile account** (for downloading Openbook)
  * to obtain Sharefile access, please e-mail openbook@talligent.com

It creates/assigns:
* a single instance running all pieces of Openbook
* a floating IP from the public pool
* an openbook-group security group

The ouptut will contain the Openbook user/password and UI address.

Example usage
-------------
On a machine with the python-heatclient (source your keystonerc or include your openstack credentials in the heat call)

This assumes that you have a public network named 'public' (default), otherwise, include the name/id
of the public network in the parameters "public_net=<public network id/name":

```
heat -d -v stack-create openbook-single-stack \
  -u https://raw.githubusercontent.com/Talligent/openbook-heat/master/openbook-single.yaml \
  -P "key_name=<keypair name>;image=<image name/id>;private_net=<private network name/id>;sharefile_user=<sharefile username>;sharefile_pass=<sharefile password>"
```


openbook-cluster
================
Similar requirements to the openbook-single template. This template deploys a seven node architeture
consisting of a three-node galera cluster, three (default) tomcat servers, and an haproxy sitting in
front of the tomcat servers.  It currently uses the load balancing as a service feature of OpenStack
for the galera cluster.

Example usage
-------------
On a machine with the python-heatclient (source your keystonerc or include your openstack credentials in the heat call)

This assumes that you have a public network named 'public' (default), otherwise, include the name/id
of the public network in the parameters "public_net=<public network id/name":

```
heat -d -v stack-create openbook-single-stack \
  -u https://raw.githubusercontent.com/Talligent/openbook-heat/master/openbook-cluster.yaml \
  -e https://raw.githubusercontent.com/Talligent/openbook-heat/master/lib/env.yaml \
  -P "key_name=<keypair name>;image=<image name/id>;private_net=<private network name/id>;sharefile_user=<sharefile username>;sharefile_pass=<sharefile password>"
```

To reduce deployment time and calls to Sharefile, the Openbook zip can be downloaded and hosted
locally (reachable by the OpenStack instances) for access by the Openbook nodes.  To enable this
in the templates, leave off the sharefile parameters and instead include the alt_download_url
parameter:
```
  -P "key_name=<keypair name>;image=<image name/id>;private_net=<private network name/id>;alt_download_url=<url of hosted Openbook zip>"
```
