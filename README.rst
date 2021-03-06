Rinnegan
########

   One who wields the Rinnegan is sent down from the heavens to become either a "God of Creation"
   who will calm the world or a "God of Destruction" who will reduce everything to nothingness.


Why?
****

To help with reverse engineering with complex distributed systems. Helps in
greatly decoding communications and events happening in various components
of a distributed system.

Requirements
************

Hosts
=====

* bash
* golang

Targets
=======

Privileged access without sudo is required.

Walkthrough
***********

For now: https://tunnelshade.in/blog/2018/12/rinnegan-walkthrough/

Tricks
^^^^^^

*INCOMING = PREROUTING dnat*
*OUTGOING = OUTPUT dnat*

To redirect incoming traffic from target to your reverse proxy

.. code-block::

   ./bin/rinnegan.sh "<filter>" agent iptables incoming add <some_ip_of_target> <port> <reverse_proxy_ip:reverse_proxy_port>

To delete a rule, just replace ``add`` with ``remove``

.. code-block::

   ./bin/rinnegan.sh "<filter>" agent iptables incoming remove <some_ip_of_target> <port> <reverse_proxy_ip:reverse_proxy_port>

Let us say you want to intercept traffic arriving at a web server, then redirecting incoming traffic on that web server target makes sense.
One caveat of this is all clients connecting to this server will get their traffic rerouted. Now what if you want to intercept traffic for
one particular client. This is where redirect outgoing on client instance makes sense.

.. code-block::

   ./bin/rinnegan.sh "<filter>" agent iptables outgoing add <ip_of_server> <port_of_server> <reverse_proxy_ip:reverse_proxy_port>

Just replace ``add`` with ``remove`` to remove this redirection.


Known Issues
************

This is quite unstable, so expect a bumpy ride

* Agent just smoked up too many processes, what to do? Just restart your containers/vms.
