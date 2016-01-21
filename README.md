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
* an Ubuntu 14.04 server glance image (image: defaults to 'ubuntu-14.04')
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
On a machine with the python-heatclient and this repo (source your keystonerc or include your openstack credentials in the heat call)

This also assumes that you have a single private network for your tenant named 'private' and a public network named 'public'
```
heat -d stack-create openbook-stack-01 \
  -u https://raw.githubusercontent.com/Talligent/openbook-heat/master/openbook-single.yaml \
  -e https://raw.githubusercontent.com/Talligent/openbook-heat/master/lib/env.yaml \
  -P "key_name=<your keypair name>;sharefile_user=<your e-mail>;sharefile_pass=<your sharefile password>"
```
