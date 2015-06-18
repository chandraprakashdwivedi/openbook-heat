# openbook-heat

Description
===========
This is a collection of Heat templates for deploying Talligent's OpenBook
billing/invoicing and customer lifecycle management software for OpenStack.
One template is for OpenBook evaluation purposes (openbook-single.yaml),
while the other is for deploying a production-ready OpenBook (openbook-cluster.yaml)

openbook-single
===============
This template deploys a single instance (m1.medium) into an OpenStack
environment.  It requires:
* an existing key pair (key_name)
* an OpenStack flavor that matches the m1.medium specs (flavor: defaults to 'm1.medium')
* an Ubuntu 14.04 server glance image (image: defaults to 'ubuntu-14.04')
* a private neutron network (private_net: defaults to 'private')
* a public/external neutron network (public_net: defaults to 'public')
* **username and password to the Talligent Sharefile account** (for downloading OpenBook)

It creates/assigns:
* a single instance running all pieces of OpenBook
* a floating IP from the public pool
* an openbook-group security group

The ouptut will contain the OpenBook user/password and UI address.