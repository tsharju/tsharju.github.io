---
author: teemuharjublog
comments: true
date: 2008-12-14 20:15:38+00:00
layout: post
slug: setting-your-srv-records-straight-for-xmpp
title: Setting your SRV records straight for XMPP
wordpress_id: 194
categories:
- XMPP
tags:
- slicehost
- XMPP
---

I've noticed that DSN SRV records are something that are not very common knowledge among even some of the really techy people. Well, the reason is probably that most people never need to know what they actually are. I'm gonna explain them here briefly and show how I set the DNS SRV records for my XMPP server hosted at [Slicehost.com](http://slicehost.com).

Ok, so what these DNS SRV records are. If you're familiar with the DNS MX record you know that it is used to indicate where is the email server that is hosting a certain domain. For example, I have domain _service.com_ and I'm running my webserver at _www.service.com_ and my email server is at _smtp.service.com_. Naturally, I would want my users to have email addresses like _user@service.com_. Here I would set the DNS MX record to point to _smtp.service.com_ so when email servers communicate with each other they can ask from DNS server that _"where is the email server for domain service.com?"_.

DNS SRV records were specified for a bit more general purpose than MX records that can be used with email only. You might have XMPP server at _xmpp.service.com_ and would like your users have also XMPP address of the format _user@service.com_. This is how the SRV records for the XMPP look like. This is an example of SRV records I've set for my XMPP server at _teemu.im_.

`
_xmpp-client._tcp.teemu.im. 82698 IN	SRV	10 0 5222 teemu.im.
_xmpp-server._tcp.teemu.im. 86400 IN	SRV	10 0 5269 teemu.im.
`

What there actually mean? Well, the `_xmpp-client._tcp.teemu.im.` line is the line that XMPP clients use to ask from DNS that _"where I can find the XMPP server for domain teemu.im?"_. The DNS server responds that _"the XMPP server can be found from host teemu.im port 5222"_. The other line that has `_xmpp-server._tcp.teemu.im.` is used by the XMPP servers when they talk to each other using the XMPP server-to-server protocol, similarly as the email servers use the MX records to find out information about each other.

The SRV records can also be used to do "poor mans load balancing" by using the priority and weight attributes, but I won't go in to that now. Instead, I'll show you how you can configure DSN SRV records on [Slicehost.com](http://slicehost.com) server. This is because I use Slicehost, but the same principles apply for other hosting providers as well.

First, go to your [SliceManager](http://manage.slicehost.com) and login. There choose the "DNS" tab and you should see something like this:

[caption id="attachment_211" align="alignnone" width="560" caption="Slicehost's SliceManager DNS Zones Tab"]![Slicehost's SliceManager DNS Zones Tab](http://blog.teemu.im/wp-content/uploads/2008/12/slicehost_dns_zones-560x83.png)[/caption]

Now click "Records" and then "new record". Fill the form as shown below:

[caption id="attachment_214" align="alignnone" width="431" caption="Slicehost SliceManager new DNS SRV record for XMPP clients"]![Slicehost SliceManager new DNS SRV record for XMPP clients](http://blog.teemu.im/wp-content/uploads/2008/12/slicehost_new_record_xmpp_client.png)[/caption]

Note that the value in "Name" field starts with underscore although it is not visible in the screenshot.

Do the same for the server-to-server protocol, except for the name set `_xmpp-server._tcp.domain.com.` and for the data use `0 5269 domain.com.`.

You can use tool called `dig` for testing your configuration. Type the following line to see if your configuration is correct:

`dig @ns1.slicehost.net SRV _xmpp-client._tcp.domain.com`

The result should be:

`
;; ANSWER SECTION:
_xmpp-client._tcp.domain.com. 86400 IN	SRV	10 0 5222 domain.com.
`

By the way... when configuring DNS it is good to set the time-to-live value to something relatively small. In case you make mistakes, you don't need to wait until the DNS record will be updated. Other good practice is to use the `@nameserver.domain.com` parameter for `dig` command so that the entry does not propagate to other DNS servers and you can change it pretty much when ever you want.
