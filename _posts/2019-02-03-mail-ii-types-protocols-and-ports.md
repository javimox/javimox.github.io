---
title: "Mail II: Types, protocols and ports"
categories: 
  - sysadmin
tags:
  - mail
  - smarthost
  - smtp
  - pop3
  - imap
  - tls
  - starttls
date: 2019-02-03T20:00:15+02:00
last_modified_at: 2019-02-03T21:15:40+02:00
---

## Introduction

This is a compendium of some _old_ notes, partly mine, partly from external sources.  
In the [references](#references) section there are some links to the sources that could have been identified.

## SMTP Relays / Smart-Hosts

In general, both are mail relays, and a mail relay is just a server that passes mail to another mail server, via SMTP, rather than a server that offers mailbox service to end users via POP3/IMAP/HTTP.

A smarthost is a mail relay that is specialized to deal with outbound email.  
If you have a private LAN, you may want to control the flow of outbound email, and prevent "any old server" from being able to deliver email to the internet, or perhaps your internal systems only resolve internal DNS and can't resolve hosts or domain MX records for systems "out there on the interwebs".

In a case like this, you might designate a single host as the Smarthost. All the other machines would in turn blindly send any outbound email to the Smarthost. The smarthost would have the ability to resolve hosts and domain MX records on the internet, and would be permitted by firewall/acl/iptables/whatever to communicate to other hosts on **port 25**, or **port 587**, to deliver outbound email.

The other common use of a mail relay is with inbound email. If you run a large organization, with thousands or hundreds of thousands of users, writing email to block storage can consume a tremendous amount of time and resources.

If you only had one server to do this, it would quickly become bogged down. If you have multiple servers, serving a subset of users each, you would have to change each user's email domain to be distinct for that user.  
Those workarounds become fairly inconvenient rather quickly. 

The solution for this is a single MX record for your domain, which might resolve (by load balancing, or DNS round-robining) to multiple mail-relay servers. These mail-relays would be configured to accept email for any users in the domain, while filtering SPAM, then it would consult it's own policies/maps to determine to which mailbox server the email needs to be forwarded to to reach the end-user's mailbox.

```
userA => server1, userB => server2 ... userN => serverN
```

This allows the servers that do the heavy lifting of receiving email from the internet for all the users to rapidly forward them off, while the mailbox servers having a lower individual volume, are able to incur the resource penalties of writing messages to disk, without becoming a bottleneck.

## Port Numbers

Here a list of the most important ports in the mail systems.

* **Mail submission** should be done on **port 587** instead of 25 (transmission)

* **SMTP** uses **port 25**
  * implicit **SSL/TLS encrypted SMTP** uses **port 465**

* **POP** uses **port 110**
  * implicit **SSL/TLS encrypted POP** uses **port 995**

* **IMAP** uses **port 143**
  * implicit **SSL/TLS encrypted IMAP** uses **port 993**

At some point, it was decided that having 2 ports for every protocol was wasteful, and instead you should have 1 port that starts off as plaintext, but the client can upgrade the connection to an SSL/TLS encrypted one. This is what STARTTLS was created to do.

Mechanisms were added to each protocol to tell clients that the plaintext protocol supported upgrading to SSL/TLS (i.e. STARTTLS), and that they should not attempt to log in without doing the STARTTLS upgrade.

Many sites now disable plain IMAP (port 143) and plain POP (port 110) altogether so people must use an SSL/TLS encrypted connection. By disabling ports 143 and 110, this removes completely STARTTLS as even an option for IMAP/POP connections.

## TLS

In 2018 it was decided to change yet again the [RFC 8314](https://tools.ietf.org/html/rfc8314#section-3), and recommend using implicit **TLS** over port **465** to:

> encourage more widespread use of TLS and to also encourage greater consistency regarding how TLS is used, this specification now recommends the use of Implicit TLS for POP, IMAP, SMTP Submission, and all other protocols used between an MUA and an MSP (Mail Service Provider).

Port 465 and 587 are both **valid** ports for a mail submission agent (MSA). Port 465 requires negotiation of TLS/SSL at connection setup and port 587 uses STARTTLS if one chooses to negotiate TLS.

Again, given the very long life of client software, it's likely to take decades before port 587 can be discontinued, so might be continued offering both **implicit TLS over port 465** and **opportunistic STARTTLS over port 587** for the foreseeable future.

## <a name="references"></a>References

* [RFC 1939](http://www.faqs.org/rfcs/rfc1939.html), which describes POP3.
* [RFC 2060](http://www.faqs.org/rfcs/rfc2060.html), which describes IMAP.
* [RFC 2476](http://www.faqs.org/rfcs/rfc2476.html), which describes the role of an MSA.
* [RFC 2821](http://www.faqs.org/rfcs/rfc2821.html), which describes SMTP.
* [Sendmail](http://www.sendmail.org/), the oldest Unix MTA.
* [Qmail](http://www.qmail.org/), another MTA.
* [Postfix](http://www.postfix.org/), another MTA.
* [Exim](http://www.exim.org/), another MTA.
* [Procmail](http://www.procmail.org/), a popular MDA for use with sendmail, or for filtering messages.
* [Fetchmail](http://catb.org/~esr/fetchmail/), a POP3/IMAP client that retrieves e-mail from another host and then delivers it to SMTP (or procmail) on the localhost.
* [Teoría del correo (Spanish)](https://cursosasir.files.wordpress.com/2014/06/teoria_correo.pdf)
* [How E-Mail works](https://howto.lintel.in/how-does-email-work/)
* [An Introduction to Internet E-Mail](http://wooledge.org/~greg/mail.html)
* [Fastmail Docs: SSL, TLS, and STARTTLS](https://www.fastmail.com/help/technical/ssltlsstarttls.html)
* [Postfix Docs: TLS](http://www.postfix.org/TLS_README.html)


Jump to [Mail I: Agents](/sysadmin/mail-i-service-elements/)

Jump to [Mail III: Local mail storage](/sysadmin/mail-iii-local-mail-storage)
